# Refactor Controller Logic to Services

An Angular component controller has two primary functions; it is responsible for consuming just enough data to satisfy the view that it controls and communicate user events back to a service for processing.

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