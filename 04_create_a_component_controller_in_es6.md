# Create a Component Controller in ES6

The simplest version of an Angular 2 component is just an HTML template and a selector that allows us to attach our component to the DOM. As an organizational mechanism, this is a great approach but eventually we will need to attach some presentational logic to some of our components. This is pretty easy to do in Angular 2 because all of our components require an ES6 class to decorate. In fact, our ES6 class may hold nothing at all but it is always there.

```javascript
import {Component, OnInit} from '@angular/core';
import {MessageService} from '../shared';

@Component({
  moduleId: module.id,
  selector: 'home',
  templateUrl: 'home.component.html',
  styleUrls: ['home.component.css']
})
export class HomeComponent {}
```

And when we need to add functionality, we simply augment our existing class to do what we need.

```javascript
export class HomeComponent implements OnInit {
  title: string = 'Home Page';
  body:  string = 'This is the about home body';
  message: string;

  constructor(private messageService: MessageService) { }

  ngOnInit() {
    this.message = this.messageService.getMessage();
  }

  updateMessage(m: string): void {
    this.messageService.setMessage(m);
  }
}
```

In Angular 1, the wiring is a bit different but we are still staying true to the theme that the steps are almost identical. In our component definition object below, we have a simple component that currently only has a template. It is worth noting that in Angular 2, we can explicitly define styles on our component but the best we can do in Angular 1 is to implicitly "attach" them to our component by importing our styles when we define the component. 

```javascript
import template from './categories.html';
import './categories.styl';

const categoriesComponent = {
  template
};

export default categoriesComponent;
```

To add a controller to our component, we will first create our **CategoriesController** which is just an ES6 class. We will get into specific details of what is happening in this class in later sections but for now, just make note of how close it compares to the Angular 2 component class above.

```javascript
class CategoriesController {
  constructor(CategoriesModel) {
    'ngInject';

    this.CategoriesModel = CategoriesModel;
  }

  $onInit() {
    this.CategoriesModel.getCategories()
      .then(result => this.categories = result);
  }

  onCategorySelected(category) {
    this.CategoriesModel.setCurrentCategory(category);
  }

  isCurrentCategory(category) {
    return this.CategoriesModel.getCurrentCategory() &&
      this.CategoriesModel.getCurrentCategory().id === category.id;
  }
}

export default CategoriesController;
```

Now that we have created our controller class, the only thing left to do is to wire it up. We will first import the controller and then add it to the component definition object. Because we are using a classical based approach, we will also define the **controllerAs** property to our configuration object.

```javascript
import template from './categories.html';
import controller from './categories.controller';
import './categories.styl';

const categoriesComponent = {
  template,
  controller,
  controllerAs: 'categoriesListCtrl'
};

export default categoriesComponent;
```

The connection that I want to you to make here with is that we are approximating the **@Component** decorator in Angular 1 using a component configuration objection connect our controller, template and in the loosest sense of the word, our styles.