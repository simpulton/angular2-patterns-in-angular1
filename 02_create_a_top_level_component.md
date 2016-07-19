# Create a Top Level Component

The entry point to an Angular 2 application is a top level component that we use to attach all of our other components onto. In the code below, we are bootstrapping our Angular 2 application using the **boostrap** method and passing in **AppComponent** as our top level component. 

```javascript
import { bootstrap } from '@angular/platform-browser-dynamic';
import { ROUTER_PROVIDERS } from '@angular/router';
import { AppComponent, environment } from './app/';

bootstrap(AppComponent, [ROUTER_PROVIDERS]);
```

In order for our **AppComponent** to to be instantiated in our application, we need to add it to our **index.html** page via its selector which in this case is **app**.  

```html
<body>
  <app>Loading...</app>

  <script>
    System.import('system-config.js').then(function () {
      System.import('main');
    }).catch(console.error.bind(console));
  </script>
</body>
```



```javascript
import angular from 'angular';
import appComponent from './app.component';
import CommonModule from './common/common';
import ComponentsModule from './components/components';

angular.module('app', [
    CommonModule.name,
    ComponentsModule.name
  ])
  .component('app', appComponent)
;
```

```html
<body ng-app="app" ng-strict-di ng-cloak>

  <app>
    Loading...
  </app>

  <script src="bundle.js"></script>
</body>
```