# Isolating State Mutations in Angular Components

If we are moving to an immutable, unidirectional data flow then how do we handle mutable operations like forms? Mutable state in itself is not a bad thing as long as we can isolate it. It is shared mutable state causes all manner of evil to be called down upon our application. When it comes to forms in Angular, the solution to this problem is so simple, it almost feels like sleight of hand.

Let us start with an example that not only has mutable state but it is shared between components and then work to solve the issue at hand. In our component below, we are sending in a **bookmark** object for editing. When we are done, we can either **save** our changes or **cancel** our changes.

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

In our template, we are binding our form to the **boomkark** object that we passed in via our **bindings**. What is going to happen the moment we start to edit our **bookmark** object? Because we are passing in a reference to an object that exists outside of our component, when we mutate our object, it will change that object on the parent as well. Well, great! So how do we back out of this if we change our mind? We have already changed the object. 

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

What we need to do is find a way to give our form a brand new object that matches the object we want to edit so that we can change it at will without affecting things upstream. If we change our mind, we can just throw this object away and no one will be the wiser. So how do we create this object and more importantly, **when** do we create this object?

This is where component lifecycle hooks saves the day yet again! Using the **$onChanges** hook, we can be notified every time an object we are binding to changes. And within that event hook, we can use **Object.assign** to create a copy of our **bookmark** object and assign it to **this.editedBookmark**.

```javascript
class SaveController {
  $onChanges() {
    this.editedBookmark = Object.assign({}, this.bookmark);
  }
}

export default SaveController;
```

And then we update our form to bind to **saveBookmarkCtrl.editedBookmark** which effectively isolates our state mutations to our component.

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