# Initialize Components with Lifecycle Hooks

Components have an internal lifecycle that is managed by Angular as the component is added to the DOM, its bindings are updated, the component is removed from the DOM, etc. Angular exposes these events as lifecycle hooks to allow us to perform additional logic over the course of our application. 

There are quite a few practical reasons to use lifecycle hooks but the one I want to focus on here is the separation of construction from initialization. Constructors should contain no initialization logic as it can lead to some unpredictable behavior. A component's constructor can be called before its bindings have been initialized as they may depend on some asynchronous operation in its parent component. 


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

```javascript
export class HomeComponent implements OnInit {
  message: string;

  constructor(private messageService: MessageService) { }

  ngOnInit() {
    this.message = this.messageService.getMessage();
  }
}
```