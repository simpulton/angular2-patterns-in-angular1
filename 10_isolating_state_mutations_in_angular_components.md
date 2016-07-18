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

```html
<div class="save-bookmark">
	<form ng-submit="saveBookmarkCtrl.save({bookmark:saveBookmarkCtrl.bookmark})" >
		<div class="form-group">
			<label>Bookmark Title</label>
			<input type="text" ng-model="saveBookmarkCtrl.bookmark.title">
		</div>
		<div class="form-group">
			<label>Bookmark URL</label>
			<input type="text" ng-model="saveBookmarkCtrl.bookmark.url">
		</div>
		<button type="submit">Save</button>
		<button type="button" ng-click="saveBookmarkCtrl.cancel()">Cancel</button>
	</form>
</div>
```

```javascript
class SaveController {
  $onChanges() {
    this.editedBookmark = Object.assign({}, this.bookmark);
  }
}

export default SaveController;
```

```html
<div class="save-bookmark">
	<form ng-submit="saveBookmarkCtrl.save({bookmark:saveBookmarkCtrl.editedBookmark})" >
		<div class="form-group">
			<label>Bookmark Title</label>
			<input type="text" ng-model="saveBookmarkCtrl.editedBookmark.title">
		</div>
		<div class="form-group">
			<label>Bookmark URL</label>
			<input type="text" ng-model="saveBookmarkCtrl.editedBookmark.url">
		</div>
		<button type="submit">Save</button>
		<button type="button" ng-click="saveBookmarkCtrl.cancel()">Cancel</button>
	</form>
</div>
```