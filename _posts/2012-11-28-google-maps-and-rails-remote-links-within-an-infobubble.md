---
layout: post
title: "Google maps and rails remote links within an InfoBubble"
description: "Google maps stops events from propagating, breaking things like rails remote links and whatever delegated events handlers you have defined. So I've written a small snippet that lets you copy all the events to a container within the Bubble."
category: "rails"
tags: ["rails", "jquery", "google maps", "delegated events"]
---
{% include JB/setup %}

_tl;dr_ jQuery delegated events like `$(body).on(’click’, ‘.popuplink’, function(e){…})` don’t work within most info Overlays on a google map, one of the things this breaks is the rails remote link/post magic. A solution that works in a lot of these cases can be found at the bottom of this post.

A little while ago we needed to add a map with markers for the first time. To a website for the bloody Beatles! Now how cool is that? They needed it done in a week, so convincing them Rails was the right option was easy.

So I got a google maps key, added a few lines of code to an empty page and blammo, got a marker and a popup when you click it. Thinking I might actually get a weekend off, I was as happy as a nerd who finds the stuff really just works.

Like a thousand people before me I naturally used an InfoWindow for the popup, but quickly found that it is almost impossible to style, since you don’t have any say on the border of it, nor any classes to tweak.  
So I started searching for other implementations and found the [InfoBubble](http://google-maps-utility-library-v3.googlecode.com/svn/trunk/infobubble/examples/example.html), added some classes to the generated html and was running smoothly again.  
Until I tried to use a twitter intent from within the bubble - There’s a jquery event handler with a class selector on the body that makes any twitter intent open in a popup - and found the link opening in the current window.  
Turned out the bubble was stopping all click events from bubbling up, so that google maps wouldn’t handle it. The only solution the internet brought me was to break down the blockade, but that only works if you don’t have a click-listener on the google map, which I was going to need. So instead I took a shortcut and just added the event handler to the bubble after creating it. Easy, quick, slightly hackish, but I could live with it.  
15 minutes later I added a remote posting form that returned javascript to replace the contents of the bubble and ... hit the exact same problem of course.

Since you can’t add the rails click handlers to different element without rewriting jquery_ujs, I was in need of a cleaner solution. Finding out a bit more on how the jquery delegated events work seemed like good option.  
Turns out all the information about the events can be found in

    $._data(body, 'events')

So I decided to copy all the delegated click events from the body to my newly created bubble node.

    $.each($._data(body, 'events')['click'], function(j, jqe_spec) {
      if (jqe_spec.selector != undefined) // delegated event handlers are those with a selector.
        $._data(bubble_element, 'events')['click'].push( jqe_spec )
    });


This simple approach worked like a charm except for ie8 and below, since jquery needs to do a lot of patching there under the hood for the events and rails needs to add in ajax sending the right headers and accepting javascript responses. Luckily recreating the events the normal way is almost easier. So my end result was this:

{% gist 4160154 %}
