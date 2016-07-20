# Create Lightweight Controllers by Binding to Models

If our goal is to build lightweight controllers, how we do surface business logic from our services into our component? A simple technique for this is to bind directly to our services so that we can expose them in our templates. 

For instance, we have extracted out the logic around getting the currently selected category into the **CategoriesModel** service. Though the logic is extracted, we still need it available in our controller so that we can use it in a template filter. We can accomplish this by creating a **getCurrentCategory** property on our controller and assigning a reference to **this.CategoriesModel.getCurrentCategory**.

```javascript
class BookmarksController {
  //...
  
  $onInit() {
    this.BookmarksModel.getBookmarks()
      .then(bookmarks => this.bookmarks = bookmarks);

    this.getCurrentCategory = 
      this.CategoriesModel.getCurrentCategory.bind(this.CategoriesModel); // Lexical scope! :(
  }
}

export default BookmarksController;
```

One thing to be careful of is that in some cases, we will lose our lexical scope reference which will cause things to break. This disconnect is why we will explicitly force that reference by adding **.bind(this.CategoriesModel)** to our assignment. Without adding **bind**, **CategoriesModel.getCurrentCategory** gets re-rescoped to **BookmarksController**.

```html
<div class="bookmarks">
    <div ng-repeat="bookmark in bookmarksListCtrl.bookmarks 
    | filter:{category:bookmarksListCtrl.getCurrentCategory().name}">
        <button type="button" class="close">&times;</button>
        <button type="button" class="btn btn-link">
            <span class="glyphicon glyphicon-pencil"></span>
        </button>
        <a href="{{bookmark.url}}" target="_blank">{{bookmark.title}}</a>
    </div>
</div>
```

With our reference setup appropriately, we can then add a filter to our template that is bound to our controller which in reality is bound to our model.
