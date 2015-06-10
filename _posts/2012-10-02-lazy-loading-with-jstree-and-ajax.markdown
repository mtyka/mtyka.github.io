---
layout: post
title:  "Lazy Loading with JSTree and AJAX"
date:   2012-10-02 00:03:44
categories: code 
---

I’ve been putting to gether a little web app recently and wanted to create a jsTree to display data held on the server. Since it’s potentially a *lot* of data I wanted each level of the tree to load just-in-time, i.e. do what’s called “lazy-loading”. First I coded a version that did this manually, i.e. I added a hook for when each node was openend and then use jQuery to issue an AJAX call to the server to request all the children of that node ID. That worked but felt awkward and, most importantly, because  jsTree only lets you add one new child at a time, was slow.

A little looking around on the web revealed that infact jsTree has a built-in way to do ajax loading. This worked well but the problem was that, because the first load would only load the top most level of the tree, jstree had no notion that each node potentially had children and thus failed to put the little “+” sign at each node junction. Thus no click events would be captured and jstree would never attempt to open a node and load more children. It took a whiel tow ork out what’s going on and none of the examples on the web had what I needed. So here’s the solution:

{% highlight javascript %}
 $("#treeCell").jstree({
      "json_data" : {
        "ajax" : {
            "url" : function( node ){
                      // lets figure out the url - this function needs to
                      // return the formed URL. If "node" is "-1" then
                      // this is the top of the hierarchy and there's no parent
                      // key. Otherwise, node is a jquery object of
                      // the respective node from which we can fish out the
                      // metadata needed. (added below at "metadata" : op )
                      if( node == -1 ){
                        return "/list?parentkey="
                      } else {
                        return "/list?parentkey=" + node.data( "key" );
                      }
                    },
            "type" : "get",  // this is a GET not a POST
            "success" : function(ops) {
                  // this is called when the AJAX request is successful. "ops"
                  // contains the returned data from the server, which in
                  // my case is a json object containing an array of objects.
                  data = []
                  // go through data and create an array of objects to be given
                  // to jsTree just like when you're creating a static jsTree.
                  for( opnum in ops ){
                    var op = ops[opnum]
                    node = {
                        "data" : op.info,
                        "metadata" :  op ,
                        // THIS LINE BELOW IS THE MAGIC KEY!!! This will force
                        //  jsTree to consider the node
                        // openable and thus issue a new AJAX call hen the
                        // user clicks on the little "+" symbol or
                        // whatever opens nodes in your theme
                        "state" : "closed"
                    }
                    data.push( node );
                  }
                  return data; // this will return the formed array
                               // "data" to jsTree which will turn
                               // it into a list of nodes and
                               // insert it into the tree.
            }
         },
      },
      "core" : {"html_titles" : true,  "load_open" : true },
      "plugins" : [ "themes", "json_data", "ui", "cookies", "crrm", "sort" ]
  });
{% endhighlight %}

The key was the "state" : "closed" line.
 

