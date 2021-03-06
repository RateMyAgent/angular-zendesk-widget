# angular-zendesk-widget [![Build Status](https://travis-ci.org/CrossLead/angular-zendesk-widget.svg?branch=master)](https://travis-ci.org/CrossLead/angular-zendesk-widget)
Angular 1.x wrapper for the [Zendesk Web Widget](https://support.zendesk.com/hc/en-us/articles/203908456-Using-Web-Widget-to-embed-customer-service-in-your-website)

For Angular 2/4+, please see https://github.com/AlisonVilela/ngx-zendesk-webwidget.

## WHY DID WE FORK THIS?

This repo has been forked to not `require('angular')` which was causing us issues of angular being pulled in twice into our bundle. It now uses `window.angular` instead. 

It also now does the following in [angular-zendesk-widget.js](./dist/angular-zendesk-widget.js): 

```js
export default angular.module('zendeskWidget').name;
```

## Installation
Via bower:
```bash
$ bower install angular-zendesk-widget
```
Or grab the [latest release](https://github.com/CrossLead/angular-zendesk-widget/releases) and add the JavaScript directly:
```html
<!-- Minified -->
<script type="text/javascript" src="dist/angular-zendesk-widget.min.js"></script>
<!-- Unminified -->
<script type="text/javascript" src="dist/angular-zendesk-widget.js"></script>
```

## Usage
First, you'll need to setup the widget during the Angular configuration phase using the `ZendeskWidgetProvider`:
```js
angular.module('myApp', ['zendeskWidget'])
  .config(['ZendeskWidgetProvider', function(ZendeskWidgetProvider) {
    ZendeskWidgetProvider.init({
      accountUrl: 'crosslead.zendesk.com'
      // See below for more settings
    });
  }]);
```
Then simply inject the `ZendeskWidget` service, call `loadWidget()` and then use it to call any of the [Web Widget API methods](https://developer.zendesk.com/embeddables/docs/widget/api):

JavaScript:
```js
angular.module('myApp')
  .controller('MyAppCtrl', ['ZendeskWidget', function(ZendeskWidget) {
	  $scope.doCustomWidgetStuff = function() {
        ZendeskWidget.loadWidget();

        ZendeskWidget.identify({
          name: $scope.currentUser.displayName,
          email: $scope.currentUser.email,
          externalId: $scope.currentUser._id,
        });
        ZendeskWidget.activate({hideOnClose:true});
	  };
  }]);
```
HTML:
```html
<div ng-app="myApp" ng-controller="MyAppCtrl">
	<a ng-click="doCustomWidgetStuff()">Click me</a>
</div>
```
# Settings
The following are all the settings that you can pass to `ZendeskWidgetProvider.init()`:
```js
ZendeskWidgetProvider.init({
  // Your Zendesk account URL. Required
  accountUrl: 'crosslead.zendesk.com',
  // Callback to execute after the Zendesk Widget initializes
  // but before the page finishes loading. Probably the best
  // example from the Zendesk docs is hiding the widget initially (see
  // https://developer.zendesk.com/embeddables/docs/widget/api#ze.hide).
  // Note you do **not** need to wrap your calls in an extra `ze()` closure
  beforePageLoad: function(zE) {
    zE.hide();
  }
});
```
