# Use ES6 for Classes and Modules

Because Angular 2 is built on top of web standards, we get twice the power with half the framework. 


```javascript
import {Injectable} from '@angular/core';

@Injectable()
export class MessageService {
  private message = 'Hello Message';

  getMessage(): string {
    return this.message;
  };

  setMessage(newMessage: string): void {
    this.message = newMessage;
  };
}
```

```javascript
class MessageService {
  constructor() {
    this.message = 'Hello Message'
  }

  getMessage() {
    return this.message;
  };

  setMessage(newMessage) {
    this.message = newMessage;
  };
}

export default MessageService;
```

```javascript
import angular from 'angular';
import BookmarksModule from './bookmarks/bookmarks';
import CategoriesModule from './categories/categories';

const ComponentsModule = angular.module('app.components', [
  BookmarksModule.name,
  CategoriesModule.name
]);

export default ComponentsModule;
```