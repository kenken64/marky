marky [![Build Status](https://travis-ci.org/nolanlawson/marky.svg?branch=master)](https://travis-ci.org/nolanlawson/marky)
======

JavaScript timer based on `performance.mark()` and `performance.measure()`, providing [high-resolution
timings](https://developer.mozilla.org/en-US/docs/Web/API/User_Timing_API) as well as nice Dev Tools visualizations.

For browsers that don't support `performance.mark()`, it falls back to 
`performance.now()` or `Date.now()`. In Node, it uses `process.hrtime()`.

Quick start
----

Install via npm:

    npm install marky

Or as a script tag:

```html
<script src="https://unpkg.com/marky/dist/marky.min.js"></script>
```

Then take some measurements:

```js
var marky = require('marky');

marky.mark('expensive operation');
doExpensiveOperation();
marky.stop('expensive operation');
```

Why?
---

The [User Timing API](https://developer.mozilla.org/en-US/docs/Web/API/User_Timing_API) is [more performant](https://twitter.com/Runspired/status/811007272671293440)
than `console.time()` and `console.timeEnd()`,
and [more accurate](https://developer.mozilla.org/en-US/docs/Web/API/Performance/now) than `Date.now()`. Also, you get nice visualizations in Chrome Dev Tools:

![Chrome Dev Tools screenshot](doc/chrome.png)

As well as Edge F12 Tools:

![Edge F12 screenshot](doc/edge.png)

Plus, it records
[PerformanceEntries](https://developer.mozilla.org/en-US/docs/Web/API/PerformanceEntry) that you can access through
the standard [Performance API](https://developer.mozilla.org/en-US/docs/Web/API/Performance) (in [supported browsers](http://caniuse.com/#feat=user-timing):

```js
// get all startTimes, names, and durations
var measurements = performance.getEntriesByType('measure');
```

API
---

`marky.mark()` begins recording, and `marky.stop()` finishes recording:

```js
marky.mark('releaseTheHounds');
releaseTheHounds();
marky.stop('releaseTheHounds');
```

You can also do more complex scenarios:

```js
function setSail() {
  marky.mark('setSail');
  marky.mark('raiseTheAnchor');
  raiseTheAnchor();
  marky.stop('raiseTheAnchor');
  marky.mark('unfurlTheSails');
  unfurlTheSails();
  marky.stop('unfurlTheSails');
  marky.stop('setSail');
}
```

`marky.stop()` also returns a `PerformanceEntry`:

```js
marky.mark('manTheTorpedos');
manTheTorpedos();
var entry = marky.stop('manTheTorpedos');
```

The entry will look something like:

```json
{
  "entryType": "measure",
  "startTime": 1974112,
  "duration": 350,
  "name": "manTheTorpedos"
}
```

Browser support
----

Marky is tested in the following browsers/environments:

* IE 9+
* Safari 8+
* iOS 8+
* Android 4.4+
* Chrome
* Firefox
* Edge
* Node 4+

In Node and [browsers that don't support the User Timing API](http://caniuse.com/#feat=user-timing), `marky.stop()` will
still return an object with a `duration` and a `startTime`, but you won't be able to find it via `performance.getEntries()`.

Credits
----

Thanks to [@toddreifsteck](https://github.com/toddreifsteck) for feedback on this project and clarifications on the User Timing API.
