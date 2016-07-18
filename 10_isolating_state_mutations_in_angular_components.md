# Isolating State Mutations in Angular Components

If we are moving to an immutable, unidirectional data flow then how do we handle mutable operations like forms? Mutable state in itself is not a bad thing as long as we can isolate it. 

```javascript
import template from './save-bookmark.html';
import controller from './save-bookmark.controller';

let saveBookmarkComponent = {
  bindings: {
    bookmark: '<',
    save: '&',
    cancel: '&'
  },
  template,
  controller,
  controllerAs: 'saveBookmarkCtrl'
};

export default saveBookmarkComponent;
```