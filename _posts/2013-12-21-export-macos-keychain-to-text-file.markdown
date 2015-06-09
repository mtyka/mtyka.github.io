---
layout: post
title:  "Export MacOS keychain to text file"
date:   2013-12-21 00:03:44
categories: code 
---

How to dump a MacOS key chain into a text file: (replace login.keychain with the name of the keychain in question).

{% highlight bash %}
security dump-keychain -d login.keychain
{% endhighlight %}

Or better still pipe it directly into a GPG symmetrically encrypted file. Perfect for backing up your keys and logins if you store them in your key chain!

{% highlight bash %}
security dump-keychain -d login.keychain | gpg -c
{% endhighlight %}


