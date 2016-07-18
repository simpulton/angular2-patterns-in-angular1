# Connected and Presentational Components

The most stable component we can have is a component with no logic in it whatsoever. Using Angular bindings, we can create a component that is void of logic and its only purpose is to connect data from the parent component to its view and relay user events back up to its parent component.

```html
<ul class="nav nav-sidebar">
	<li class="category-item"
		ng-repeat="category in categoriesListCtrl.categories">
		<category-item
			category="category"
			selected="categoriesListCtrl.onCategorySelected(category)">
		</category-item>
	</li>
</ul>
```