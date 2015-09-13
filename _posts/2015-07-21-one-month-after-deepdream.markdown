---
layout: post
title:  "Deepdream/Inceptionism - recap" 
date:   2015-07-21 00:03:44
categories: code
---

# DeepDream

It's now been one month or so since we published the original [Inceptionism blog post](http://googleresearch.blogspot.com/2015/06/inceptionism-going-deeper-into-neural.html) and the associated [gallery of images](https://photos.google.com/share/AF1QipPX0SCl7OzWilt9LnuQliattX4OUCj_8EP65_cTVnBmS1jnYgsGQAieQUc1VQWdgQ?key=aVBxWjhwSzg2RjJWLWRuVFBBZEN1d205bUdEMnhB). Two weeks later we also published [the deepdream source code](https://github.com/google/deepdream). Since then I've been enjoying watching people come up with really fabulous derivatives, extensions and adaptations. Time to take a quick survey.

# Services

Installing Caffe is really quite tricky, especially on Macs so quickly several services popped up which allow anyone to post pictures and deepdreamify them:

  * [http://dreamscopeapp.com](http://dreamscopeapp.com)
  * [http://deepdream.in/](http://deepdream.in/)
  * [http://psychic-vr-lab.com/deepdream/](http://psychic-vr-lab.com/deepdream/)
  * [http://deepdream.akkez.ru/](http://deepdream.akkez.ru/)
  * [http://deepdreamit.com/](http://deepdreamit.com/)
  * [http://dreamingwith.us/](http://dreamingwith.us/)
  * [http://deepdreamr.com/](http://deepdreamr.com/)
  * [and docker images](https://registry.hub.docker.com/u/mjibson/deepdream/)
  * [DeepDreamer MacOS App](http://realmacsoftware.com/deepdreamer) 

# Derivatives / Exploration

This whole thing went viral and of course everyone was now making their favorite pictures or most hated politians into deepdreams. But cutting through the [kitsch](http://www.psmag.com/nature-and-technology/googles-deep-dream-is-future-kitsch) several folks went deeper (no pun intended) and made some interesting forays:

  * [Comparisons of BVLC, MIT Places and CompCars](http://sprawledoctopus.com/deepdream/datasets/) 
  * [Fantastic single-class visualizations](http://auduno.com/post/125362849838/visualizing-googlenet-classes) by Audun m. Øygard
  * [Systematic exploration](https://www.flickr.com/photos/quasimondo/sets/72157655587555166) by @quasimondo
  * [Side-by-side comparisons](https://www.youtube.com/watch?t=16&v=32GnAI3nrsQ) of different networks by [Ville-Matias Heikkilä](http://www.pelulamu.net/viznut/)
  * [Exploring layer by layer](https://www.youtube.com/watch?v=dbQh1I_uvjo)
  * [Exploring deepzooms layer by layer](https://users.ics.aalto.fi/perellm1/deep_dreams.shtml) 
  * [DeepDreamAnimator](https://github.com/samim23/DeepDreamAnim) by @samim who made some great [videos](https://vimeo.com/133275555).
  * [Surreal art](http://gizmodo.com/this-human-artist-is-making-hauting-paintings-with-goog-1716597566) by C.M. Kosemen
  * [Interesting thoughts and experiments](https://medium.com/@memoakten/deepdream-is-blowing-my-mind-6a2c8669c698) by Memo Akten
  * [DeepDream typeset](http://gwiozda.net/content/ddAlphabet.php) - Gorgeous!
 
And finally folks started also training novel networks too on new data sets, which is really the way to get more out of DeepDream.
Some really nice work here by [Josh Nimoy](http://jtnimoy.com/)

  * [Deepdream: Avoiding Kitsch](http://jtnimoy.com/blogs/projects/50616707-deepdream-avoiding-kitsch)



