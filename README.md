## AppCacheManager

Managing multiple cache manifests on a single page using IFrames.

### Synopsis

````javascript
// sorry for the global, but the iframes have to communicate with the parent
window.appCache = new AppCacheManager({
  manifests: ["assets", "images", "i18n.en"]
  // Just in case you don't like window.appCache or have more than one instance
  // specify the name of the variable
  // , varName: "appCache" 
});

// the AppCacheManager aggregates the individual applicationCache events
// following the rules from http://www.w3.org/TR/2011/WD-html5-20110525/offline.html#appcacheevents
$(window.appCache).bind("cached", function() { /* ... */ });
````

### Why

Authors can provide a manifest which lists the files that are needed for the Web application to work offline and which causes the user's browser to keep a copy of the files for use offline.
There are, however, a few caveats that limit the usability of this feature:

* One manifest per document

* Each document maintains its own cache

* An update to the manifest file causes all assets to be requested

* No dynamic loading of manifests

* A document with a manifest behaves differently

### How

This class tries to solve these issues by providing an abstraction between the web site and the offline storage layer provided by the browser.
Instead of stuffing all files in a single manifest file, the offline content can be sliced and diced on a per subject basis. Imagine a news site that wants to make all its articles available for offline reading. The obvious solution is to put every article including images and assets in a single manifest file. This will cause several issues. Adding a new article to the manifest file will cause the client's browser to request all items to be requested with a Modified-Since-Header. While this won't cause too much traffic, it will takes ages to complete all requests and your server has to handle a huge amount of requests at the same time.
