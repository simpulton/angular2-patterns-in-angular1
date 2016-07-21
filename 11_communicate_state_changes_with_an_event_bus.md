# Communicate State Changes with an Event Bus

Angular 2 uses observables to communicate state change across the application. We do not have the same luxurious observables at the framework level in Angular 1.x, but we can approximate the pattern by creating an event bus to communicate state change.

I will generally try to bind directly to a model and let the wizardry of JavaScript and Angular keep my view in sync with my data. However, there are times when we need to be notified of a model change unrelated to our view, usually because of some indirect relationship.

A good example of this is the case of resetting a view when a new item is selected in another part of the application. For instance, if we were in the middle of editing a bookmark and a new category was selected, we would want to reset our bookmarks view so that we are not left in a weird edit state. To coordinate this, we will start by injecting **$rootScope** into our **CategoriesModel** and then broadcasting an **onCurrentCategoryUpdated** event when a new current category is set. 

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

To be clear, it is a terrifically bad idea to use **$rootScope** as a global storage device. We have services to do that for us. But there is one thing that **$rootScope** does better than anything else and that is to broadcast events reliably across our application. 

First we need to listen to our **onCurrentCategoryUpdated** event. We'll inject **$scope** into our our **BookmarksController** and then use **$scope.$on** to set up our event handler. Notice that we are using **.bind(this)** because lexical scope jumps the tracks here as well.

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

And now, whenever a category is selected, we can call **reset** on our **BookmarksController** to update our bookmarks view. 

Observables in Angular 2 are powerful because they essentially act like data collections that handle their own events and pushes out changes as they update. We can't get all of this magic in Angular 1, but our event-bus trick gets us the observer piece that we need most.