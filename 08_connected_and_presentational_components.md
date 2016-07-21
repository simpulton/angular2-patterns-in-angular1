# Connected and Presentational Components

The most stable component we can have is a component with no logic in it whatsoever. We can use Angular bindings to create components that are void of logic. Their only purpose is to connect data from the parent component to its view and relay user events back up to its parent component.

![](http://onehungrymind-45fd.kxcdn.com/books/angular2-presentational-connected.png)

For instance, we have defined a component to represent a single category item in the code below. We are defining the component's API using the **bindings** property to have a single input property of **category** and a single output event of **selected**. I have specifically used the words "input" and "output" because these are the formal semantics for this concept in Angular 2.

```javascript
import template from './category-item.html';
import './category-item.styl';

let categoryItemComponent = {
  bindings: {
    category: '<',
    selected: '&'
  },
  template,
  controllerAs: 'categoryItemCtrl'
};

export default categoryItemComponent;
```

Angular will implicitly create a controller on our component if we don't define one. This enables us to reference our bindings in the view. We simply need to determine how we want to refer to that controller by defining the **controllerAs** property.

Let's look at the entire template for our category item component. We are binding to the **name** property on the **category** object we passed, and when someone clicks on a category item, we are emitting that category item by calling **categoryItemCtrl.selected({category:categoryItemCtrl.category})**.

```html
<div class="categoryItem"
    ng-click="categoryItemCtrl.selected({category:categoryItemCtrl.category})">
    {{categoryItemCtrl.category.name}}
</div>
```

Let us expand our view just a bit by examining our component's relationship to its parent. We add our category item component to the parent component by way of an **ngRepeat**. We pass in a **category** item, listen for the **selected** event and call **onCategorySelected** from the parent component.

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

we then determine what action we want to take with our selected component in the parent controller. In most cases, the next step is a further delegation by handing off processing to a service.

```javascript
class CategoriesController {
  constructor(CategoriesModel) {
    'ngInject';

    this.CategoriesModel = CategoriesModel;
  }

  onCategorySelected(category) {
    console.log('CATEGORY SELECTED', category);
  }
}

export default CategoriesController;
```

With that example in mind, I would like to pose a question. How would a person test our category item component below?

```javascript
import template from './category-item.html';
import './category-item.styl';

let categoryItemComponent = {
  bindings: {
    category: '<',
    selected: '&'
  },
  template,
  controllerAs: 'categoryItemCtrl'
};

export default categoryItemComponent;
```

A better question could be, **what** would they test? What unit of logic presents itself for an assertion? With almost complete certainty, we can know that the only way for this component to break is for Angular to break. We have not only created a very stable component but dramatically reduced our testable surface in the process.

The goal is to create a few presentational components that are responsible for consuming data from our services. We then use that data to coordinate the layout of our connected components. We want to have as few presentational components as possible while creating as many connected components as we can.