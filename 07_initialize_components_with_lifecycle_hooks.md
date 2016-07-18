# Initialize Components with Lifecycle Hooks

Angular components have an internal lifecycle that is managed by Angular as the component is added to the DOM, its bindings are updated, the component is removed from the DOM, etc. Angular exposes these events as lifecycle hooks to allow us to perform additional logic over the course of our application. 

```javascript
class CategoriesController {
  constructor(CategoriesModel) {
    'ngInject';

    this.CategoriesModel = CategoriesModel;
    this.CategoriesModel.getCategories()
      .then(categories => this.categories = categories);    
  }
}

export default CategoriesController;
```

```javascript
class CategoriesController {
  constructor(CategoriesModel) {
    'ngInject';

    this.CategoriesModel = CategoriesModel;
  }

  $onInit() {
    this.CategoriesModel.getCategories()
      .then(categories => this.categories = categories);
  }
}

export default CategoriesController;
```