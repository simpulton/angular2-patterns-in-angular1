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