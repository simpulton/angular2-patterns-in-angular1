# Communicate State Changes with an Event Bus

Angular 2 uses observables to communicate state change across the application. We do not have the same luxury of leverage observables at the framework level in Angular 1.x, but we can approximate the pattern by creating an event bus to communicate state change.

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
    this.getCurrentCategory = 
      this.CategoriesModel.getCurrentCategory.bind(this.CategoriesModel);
    this.deleteBookmark = this.BookmarksModel.deleteBookmark;
  }

  reset() {
    this.currentBookmark = null;
  }
}

export default BookmarksController;
```