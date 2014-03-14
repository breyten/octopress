---
layout: post
title: "Tracking the current page in ProPublica's Document Viewer"
date: 2014-03-14 12:54:38 +0100
comments: true
published: false
categories: [javascript]
---
I'm using ProPublica's Document Viewer in a project and I needed to figure out how to keep track of the current page the user is viewing. It turns out that you can do this, but it required a bit of hacking.

The Document Viewer is a full-fledged project and it's well structured, as it has models, events and the like. The first thing I did was figuring out if there was an event firing after the entire document was fully loaded (a la jQuery's ready event). It turns out there are two ways to do achieve this kind of functionality. The easiest is to assign a function to the `afterLoad` key in the options you pass to `DV.load`. The other way is to extend the `DV` object with an added function `afterLoad` which is automatically called if it is implemented. 

Here is sample code to register the `afterload` function via the options:

``` javascript
var CURRENT_DOCUMENT = DV.load("https://documentcloud.org/documents/" + dc_slug + ".js", {
  container : '#doc',
  width     : 660,
  height    : 900,
  zoom      : 'auto',
  showSidebar : false,
  embedded  : true,
  showAnnotations : true,
  afterLoad : function (v) {
    console.log('afterload called via options!');
  }
});
```

The code to achieve the same by extending the `DV` object is shorter. You might prefer it -- I prefer the following over defining functions is hashes:

``` javascript
DV.afterLoad = function(v) {
  console.log('DV.afterLoad has fired!');
};
```

In order to track the page view we need to register an event handler. Luckily, the document model has an array called `onPageChangeCallbacks`, which is exactly what we need. This is how you register the callback:

``` javascript
DV.afterLoad = function(v) {
  console.log('DV.afterLoad has fired!');
  CURRENT_DOCUMENT.models.document.onPageChangeCallbacks.push(
    function (){
      var currentPage = CURRENT_DOCUMENT.models.document.currentPage();
      console.log('page changed! We are on page ' + currentPage + ' now');
    }
  );
};
```

Thhe callbacks are fired when you click the previous/next button in the viewer, but also when you're scrolling, so all bases are covered.