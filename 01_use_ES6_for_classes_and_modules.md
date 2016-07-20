# Use ES6 for Classes and Modules

Because Angular 2 is built on top of web standards, we get twice the power with half the framework. This statement is a great marketing phrase but what does this mean in a practical sense? Angular 2 uses ES6 classes and modules as the primary organizational mechanisms for defining how an application should fit together. Below, we have an Angular 2 service called **MessageService** that uses modules to import functionality and export its functionality. It also uses a **class** to organize methods and properties that help this service fulfill its job. 

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

Now let us look at the same service written for Angular 1 in ES6. There are a few minor differences but for the most part, they are identical. 

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

It is for this reason that I highly recommend switching to ES6 for Angular 1 work as soon as possible. Making the switch will allow you to start leveraging Angular 2 features at a language level.

One caveat to this is that we still need to wire up our Angular 1 application using **angular.module**. When we write Angular 1 in an Angular 2 style, we need to make the distinction of modules at the language level and the framework level. For this reason, I will often have a single file that serves as the entry point for the component that I am working on that is solely responsible for wiring up **angular.module**.

Here is an example of a components module that I use as a container for all my subcomponents so that they can be injected into the top level component as a dependency.

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