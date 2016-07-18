# Create a Top Level Component

The entry point to an Angular 2 application is a top level component that we use to attach all of our other components onto. 

```javascript
import { bootstrap } from '@angular/platform-browser-dynamic';
import { ROUTER_PROVIDERS } from '@angular/router';
import { AppComponent, environment } from './app/';

bootstrap(AppComponent, [ROUTER_PROVIDERS]);
```

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

```

```html
<body ng-app="app" ng-strict-di ng-cloak>

  <app>
    Loading...
  </app>

  <script src="bundle.js"></script>
</body>
```