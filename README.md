# $applyAsync in $http

## Overview

`$applyAsync` is a small performance offering from Angular - only really affecting apps with lots of `$http` calls. Either way, it speeds things up!

## Objectives

- Describe $applyAsync in $http
- Use $applyAsync in config()

## applyAsync

We can turn on $applyAsync in our modules config.

```js
angular
	.module('app')
	.config(function ($httpProvider) {
        $httpProvider.useApplyAsync(true);
	});
```

Here we're injecting `$httpProvider` (that configures everything to do with `$http`) and turning applyAsync on. But what does it actually do?

applyAsync batches our `$http` calls that happen at a similar time into one digest cycle call. If we have two `$http` calls that happen at the same time, normally we'd be running two digest cycles - this isn't great for performance.

Instead, applyAsync will notice that these two requests are near each other and wait for them both to complete, running one digest cycle after that. This is great! It saves a lot of processing time for one little configuration object. However, this does mean there is a slight delay between making your request and then Angular making the actual HTTP request - this is because it needs to wait to see if there are any other `$http` calls to batch into one.