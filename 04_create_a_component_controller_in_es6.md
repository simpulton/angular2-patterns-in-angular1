# Create a Component Controller in ES6

The simplest version of an Angular 2 component is just an HTML template and a selector that allows us to attach our component to the DOM.

```javascript
import {Component, OnInit} from '@angular/core';
import {MessageService} from '../shared';

@Component({
  moduleId: module.id,
  selector: 'app-home',
  templateUrl: 'home.component.html',
  styleUrls: ['home.component.css']
})
```

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

```javascript
import template from './categories.html';
import './categories.styl';

const categoriesComponent = {
  template
};

export default categoriesComponent;
```

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