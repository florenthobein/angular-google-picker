angular-google-picker
=====================

Angular directive that interact with the Google API Picker :  
* [https://developers.google.com/picker/docs/](https://developers.google.com/picker/)
* [https://developers.google.com/picker/docs/](https://developers.google.com/picker/docs/)  

**Requirements:** AngularJS 1.2+

**File Size:** 1.9Kb minified


# Usage

1. Include Google client and api script in your layout  

  ```html
  <script src="http://apis.google.com/js/client.js"></script>
  <script src="http://apis.google.com/js/api.js"></script>
  ```
  
2. Include the Google Picker as a dependency for your app

  ```js
  angular.module('myApp', ['lk-google-picker'])
  ```
  
3. Configure the plugin (see below **configuration** section)  

4. Create a scope to handle files that will be selected

  ```js
  angular.module('myApp', ['lk-google-picker'])
  
  .controller('ExamplaCtrl', ['$scope', function($scope) {
     $scope.files = [];
  }]);
  ```
  
5. Add the directive to your HTML element

  ```html
  <a href="javascript:;" lk-google-picker picker-files="files">Open my Google Drive</a>
  ```
  
6. That's it, you're done!

  
You'll notice here the usage of `picker-files`. You need to pass here a scope that gonna receive the selected files. Once selected, the plugin gonna push files into it. A file is a json object that looks like : 

  ```json
  [
    {
      "id": "0B50DHrsuMky6UFlSQloxYGBxT2M",
      "serviceId": "docs",
      "mimeType": "image/jpeg",
      "name": "DSC01845.JPG",
      "type": "photo",
      "lastEditedUtc": 1409023514905,
      "iconUrl": "https://ssl.gstatic.com/docs/doclist/images/icon_11_image_list.png",
      "description": "",
      "url": "https://docs.google.com/file/d/0B50DHrsuMky6UFlSQloxYGBxT2M/edit?usp=drive_web",
      "sizeBytes": 1570863,
      "parentId": "0B50DHrsuMkx6cWhrSXpTR1cyYW8"
    },
    {
      ...
    }
  ]
  ```  


# Configuration  

In order to work, Google Picker needs to connect to the Google API using an application credentials (Api Key and client ID). For more information on how to create an application/project, please refer to [https://developers.google.com/drive/web/](https://developers.google.com/drive/web/). To do so, you'll need to configure the service. There's severals ways to do it.  


### Using configure(options)

```js
angular.module('myApp', ['lk-google-picker'])

.config(['lkGoogleSettingsProvider', function(lkGoogleSettingsProvider) {

  // Configure the API credentials here
  lkGoogleSettingsProvider.configure({
    apiKey   : 'YOUR_API_KEY',
    clientId : 'YOUR_CLIENT_ID',
    scopes   : ['https://www.googleapis.com/auth/drive', 'another_scope', 'and_another']
  });
}])
```


### Using setters  

```js
angular.module('myApp', ['lk-google-picker'])

.config(['lkGoogleSettingsProvider', function(lkGoogleSettingsProvider) {

  // Configure the API credentials here
  lkGoogleSettingsProvider.setApiKey('YOUR_API_KEY')
                          .setClientId('YOUR_CLIENT_ID')
                          .setScopes(['https://www.googleapis.com/auth/drive']);
}])
```

### Features  

The Picker use the concept of views and features that allow you to customize it. The service provider allow you to enable some features to the Picker the same way you define your API Key or client ID (using either configure or setters). 

```js
angular.module('myApp', ['lk-google-picker'])

.config(['lkGoogleSettingsProvider', function(lkGoogleSettingsProvider) {
  lkGoogleSettingsProvider.setFeatures(['MULTISELECT_ENABLED', 'ANOTHER_ONE']);
}])
```

Please refer to [https://developers.google.com/picker/docs/reference](https://developers.google.com/picker/docs/reference) for more informations. 


### Views 

Views are objects that needs to be instanciate using the namespace `google.picker.*`. That namespace is already defined in the core of the directive. In order to add views to your picker, all you need to do is to define the class that needs to be used : 

```js
angular.module('myApp', ['lk-google-picker'])

.config(['lkGoogleSettingsProvider', function(lkGoogleSettingsProvider) {
  lkGoogleSettingsProvider.setViews([
    'DocsUploadView()',
    'DocsView()'
  ]);
}])
```

**NOTE** : Views classes have some usefull methods such as `setIncludeFolders` or `setStarred` (or any other methods available). In order to use them, just chain them to the class : 

```js
angular.module('myApp', ['lk-google-picker'])

.config(['lkGoogleSettingsProvider', function(lkGoogleSettingsProvider) {
  lkGoogleSettingsProvider.setViews([
    'DocsUploadView().setIncludeFolders(true)',
    'DocsView().setStarred(true)'
  ]);
}])
```

Please refer to [https://developers.google.com/picker/docs/reference](https://developers.google.com/picker/docs/reference) for more informations. 

# License:
Licensed under the MIT license