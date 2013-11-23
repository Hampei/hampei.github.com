---
layout: post
title: "Convert all current and future textareas into Medium Editors"
description: "Medium Editor doesn't automatically handle text areas out of the box. Here's some code snippets to make all textareas into Medium Editors and keeping the textarea in sync with the resulting html."
category: ""
tags: ["Medium-Editor", "javascript"]
---
{% include JB/setup %}

I recently started another of my many many searches for a good wysiwyg editor and I came across Medium-Editor, which quickly became my new favourite: All generated code is clean, it's completely inline on your page with your pages styles and the context menu work perfectly.

The only problem is that textareas are not supported automatically and the creators feel it's not at all a needed feature and you should just fetch the innerHTML before submitting.  
In my case however I have a bunch of textareas I want to convert without unique names and even inserting new ones dynamically. This makes the glue code messy and in any case I don't like adding glue-code for something that is the main use-case of a library. 

Given that new textarea's are being added dynamically I decided to build a find method in the style of jqueries 'on' method. It looks for a selector below a target and calls a given method for each one. A MutationObserver checks if newly created elements match the selector.
Since it admin functionality we don't have to support ie8 and below which makes life so wonderful.

{% gist 7620643 %}

Next I needed a way to sync the editors html to the textarea.

{% gist 7620825 %}

And then it was just a question of using all that to find all textareas, hiding them, creating->filling->syncing->initializing the editor and we're done.

{% gist 7620806 %}
