# Communicate State Changes with an Event Bus

Angular 2 uses observables to communicate state change across the application. We do not have the same luxury of leverage observables at the framework level in Angular 1.x, but we can approximate the pattern by creating an event bus to communicate state change.

Generally, I will try to bind directly to a model and lets the forces of JavaScript keep my view in sync with my data. There are times when we need to be notified of a model change unrelated to our view because it requires additional logic.

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