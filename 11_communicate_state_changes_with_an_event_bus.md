# Communicate State Changes with an Event Bus

Angular 2 uses observables to communicate state change across the application. We do not have the same luxury of leverage observables at the framework level in Angular 1.x, but we can approximate the pattern by creating an event bus to communicate state change.

Generally, I will try to bind directly to a model and lets the forces of JavaScript and Angular keep my view in sync with my data. There are times when we need to be notified of a model change unrelated to our view because of some indirect relationship.

A good example of this is the case of resetting a view when a new item is selected in another part of the application. For instance, if we were in the middle of editing a bookmark and a new category was selected, we would want to reset our bookmarks view so that we are not left in a weird edit state. To coordinate this, we will start by injected **$rootScope** into our **CategoriesModel** and then broadcasting an **onCurrentCategoryUpdated** event a new current category is set. 

```javascript
class CategoriesModel {
  constructor($q, $rootScope) {
    'ngInject';

    this.$q = $q;
    this.$rootScope = $rootScope;
    this.currentCategory = null;
  }

  setCurrentCategory(category) {
    this.currentCategory = category;
    this.$rootScope.$broadcast('onCurrentCategoryUpdated');
  }
}

export default CategoriesModel;
```

To be clear, it is a really bad idea to use **$rootScope** as a global storage device and the reason that we have services. But there is one thing that **$rootScope** does better than anything else and that is broadcast events to the application. 


```javascript
class BookmarksController {
  constructor($scope, CategoriesModel, BookmarksModel) {
    'ngInject';

    this.$scope = $scope;
    this.CategoriesModel = CategoriesModel;
    this.BookmarksModel = BookmarksModel;
  }

  $onInit() {
    this.BookmarksModel.getBookmarks()
      .then(bookmarks => this.bookmarks = bookmarks;);

    this.$scope.$on('onCurrentCategoryUpdated', this.reset.bind(this));
  }

  reset() {
    this.currentBookmark = null;
  }
}

export default BookmarksController;
```