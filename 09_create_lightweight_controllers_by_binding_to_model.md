# Create Lightweight Controllers by Binding to Models

If our goal is to create lightweight controllers, how we do surface business logic from our services into our component?

```javascript
class BookmarksController {
  //...
  
  $onInit() {
    this.BookmarksModel.getBookmarks()
      .then(bookmarks => this.bookmarks = bookmarks);

    this.getCurrentCategory = 
      this.CategoriesModel.getCurrentCategory.bind(this.CategoriesModel);
  }
}

export default BookmarksController;
```

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

