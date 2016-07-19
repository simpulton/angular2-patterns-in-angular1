# Initialize Components with Lifecycle Hooks

Components have an internal lifecycle that is managed by Angular as the component is added to the DOM, its bindings are updated, the component is removed from the DOM, etc. Angular exposes these events as lifecycle hooks to allow us to perform additional logic over the course of our application. 

There are quite a few practical reasons to use lifecycle hooks but the one I want to focus on here is the separation of construction from initialization. Constructors should contain no initialization logic as it can lead to some unpredictable behavior. A component's constructor can be called before its bindings have been initialized as they may depend on some asynchronous operation in its parent component. 

Let us take the example below and move the **CategoriesModel.getCategories()** call out of the constructor.

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

When a lifecycle event happens to a component, Angular implicitly calls a method on that component that we can tap into. Because we want to know when a component is initialized, we can do that by adding an **$onInit** method to our component which will get called on initialization. From there, we just move our call to **CategoriesModel.getCategories()** into our new function block.

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

This works pretty much the same in Angular 2 with a slight change in the API to **ngOnInit**. We can optionally implement the **OnInit** interface which is not required but considered best practice.


```javascript
export class HomeComponent implements OnInit {
  message: string;

  constructor(private messageService: MessageService) { }

  ngOnInit() {
    this.message = this.messageService.getMessage();
  }
}
```