# Differential Loading

Demo project to show the behaviour of differential loading in Angular v12.

## The what and the why
As web developer you want to make sure your application is compatible with the majority of browsers. Even as JavaScript continues to evolve, with new features being introduced, not all browsers are updated with support for these new features at the same pace.

The code you write in development using TypeScript is compiled and bundled into ES2015, the JavaScript syntax that is compatible with most browsers. All modern browsers support ES2015 and beyond - but although Angular no longer offers support for IE11 as of v13, there are reasons to make the application accessible to those users as well. When targeting older browsers, [polyfills](https://angular.io/guide/browser-support#polyfills) can bridge the gap by providing functionality that doesn't exist in the older versions of JavaScript supported by those browsers.

Instead of shipping a single, large bundle that includes all your compiled code, plus any polyfills that may be needed differential loading only ships the necessary code that the browser needs. When differential loading is enabled the CLI builds two separate bundles as part of your deployed application.

* The first bundle contains modern ES2015 syntax. This bundle takes advantage of built-in support in modern browsers, ships fewer polyfills, and results in a smaller bundle size.

* The second bundle contains code in the old ES5 syntax, along with all necessary polyfills. This second bundle is larger, but supports older browsers.

## How it works

To figure out which polyfills are needed, [Browserslist](https://github.com/browserslist/browserslist) is used. The supported browsers can be specified in `.browserslistrc`:

```
# This file is used by the build system to adjust CSS and JS output to support the specified browsers below.
# For additional information regarding the format and rule options, please see:
# https://github.com/browserslist/browserslist#queries

# For the full list of supported browsers by the Angular framework, please see:
# https://angular.io/guide/browser-support

# You can see what browsers were selected by your queries by running:
#   npx browserslist

last 1 Chrome version
last 1 Firefox version
last 2 Edge major versions
last 2 Safari major versions
last 2 iOS major versions
Firefox ESR
#IE 11 # Angular supports IE 11 only as an opt-in. To opt-in, remove the 'not' prefix on this line.
```

You can adjust this list to match better with your own user demographics. To support IE11 we have to uncommend the respective line:

```
IE 11 # Angular supports IE 11 only as an opt-in. To opt-in, remove the 'not' prefix on this line.
```

Now we can build the application with this command:

```
ng build --prod
```

After it’s done, open up `dist/index.html` and notice the `script` tags:

```
<script src=“runtime-es2015.858f8dd898b75fe86926.js” type=“module”></script>
<script src=“polyfills-es2015.e954256595c973372414.js” type=“module”></script>
<script src=“runtime-es5.741402d1d47331ce975c.js” nomodule></script>
<script src=“polyfills-es5.405730e5ac8f727bd7d7.js” nomodule></script>
<script src=“main-es2015.63808232910db02f6170.js” type=“module”></script>
<script src=“main-es5.2cc74ace5dd8b3ac2622.js” nomodule></script>
```

We now have two runtimes, two polyfills, and two mains. Modern browsers will only run the script that is of type module. Legacy browsers like IE 11 will run the nomodule one.




