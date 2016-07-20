# Organize Your Application with Subcomponents

The atomic building block of an Angular 2 application is the component. We start with the top level component and then continue to divide our applications into smaller units of functionality as we build out subcomponents. The goal is to end up with many fine-grained subcomponents that focus on a single task.

![](http://onehungrymind-45fd.kxcdn.com/books/angular2-subcomponents.png)

In our hypothetical Angular 2 application below, we have an **ItemsComponent** that contains all of the items. We could just define our template for a single item and then iterate over our collection of items and lay them out with **ngFor**. Repeating over a block of HTML is a viable approach and probably where I would start, but the lack of encapsulation means that I can never use the template I built to display the item's details anywhere else. The next step is to extract the details template and related functionality into a subcomponent.

```javascript
import {Component} from '@angular/core';
import {ItemDetailsComponent} from './item-details';

@Component({
  moduleId: module.id,
  selector: 'items',
  templateUrl: 'items.component.html',
  directives: [ItemDetailsComponent],
  styleUrls: ['items.component.css']
})
export class ItemsComponent {}
```

Once we have created our **ItemDetailsComponent** subcomponent, we will import it into **ItemsComponent** and make it available for dependency injection by adding it to the **directives** array in the component metadata. From there, we replace the template with the new reference to our subcomponent in the HTML.

```html
<div>
  <div>
    <h2>{{ title }}</h2>
  </div>
  <div>
    {{ body }}
  </div>
</div>

<item-details *ngFor="let item of items" [item]="item"></item-details>
```

Putting components front and center of your application and letting it drive the architecture is one of the single most important concepts in Angular 2 that we can apply to our Angular 1 applications. In the example below, the syntax is slightly different, but the approach is almost identical. We have extracted out a category item component that is wrapped in the **CategoryItemModule** that we will use to inject into the **CategoriesModule**. In Angular 2, we do this at the component level via metadata but in Angular 1, we need to do this at the component level as a submodule dependency. 

```javascript
import angular from 'angular';
import categoriesComponent from './categories.component';
import CategoryItemModule from './category-item/category-item';

const CategoriesModule = angular.module('categories', [
      CategoryItemModule.name
    ])
    .component('categories', categoriesComponent)
  ;

export default CategoriesModule;
```

And from there, we update the HTML of our parent component to reference our newly minted subcomponent.

```html
<ul class="nav nav-sidebar">
    <li ng-repeat="category in categoriesListCtrl.categories">
        <category-item
            category="category"
            selected="categoriesListCtrl.onCategorySelected(category)">
        </category-item>
    </li>
</ul>
```

This approach results in a well-organized application with clearly defined boundaries around specific functionality. Another upside is that our templates become much smaller and significantly more descriptive as we are defining a DSL as we build the application.