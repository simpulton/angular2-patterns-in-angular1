# Refactor Controller Logic to Services

An Angular component controller has two primary functions; it is responsible for consuming just enough data to satisfy the view that it controls and communicate user events back to a service for processing. With this in mind, we should extract any logic that does not directly relate to the presentation of the view into a service. A common theme in Angular 2 is that everything is "just a class" and we will see how beneficial that is when we start to extract logic into services.

In the controller below, we have a collection of categories that we should refactor to a service. Because we are using ES6 classes, the structural differences between a controller and a service are almost non-existent. 

```javascript
class CategoriesController {
  constructor() {
    this.categories = [
      {"id": 0, "name": "Development"},
      {"id": 1, "name": "Design"},
      {"id": 2, "name": "Exercise"},
      {"id": 3, "name": "Humor"}
    ];
  }
}

export default CategoriesController;
```

To prove this, we can just paste the contents of the **CategoriesController** into a new file and rename the class to **CategoriesModel**. Done. 

```javascript
class CategoriesModel {
  constructor() {
    this.categories = [
      {"id": 0, "name": "Development"},
      {"id": 1, "name": "Design"},
      {"id": 2, "name": "Exercise"},
      {"id": 3, "name": "Humor"}
    ];
  }
}

export default CategoriesModel;
```

And we can see a variation of this in Angular 2 which still bears a striking resemblance to our Angular 1 version. The only difference is that in the code below, because we are using TypeScript, we are using field assignment to initialize **categories** and an access modifier to set it to **private**.

```javascript
import {Injectable} from '@angular/core';

@Injectable()
export class CategoriesModel {
  private categories = [
      {"id": 0, "name": "Development"},
      {"id": 1, "name": "Design"},
      {"id": 2, "name": "Exercise"},
      {"id": 3, "name": "Humor"}
    ];

  getCategories(): string {
    return this.categories;
  };
}
```