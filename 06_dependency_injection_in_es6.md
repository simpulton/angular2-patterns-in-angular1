# Dependency Injection in ES6

Dependency injection in an Angular application using ES6 happens at the constructor level as we pass our dependencies in as parameters. This comes with a slight nuance that will could trip up developers who are unfamiliar with working in a classical language. 

```javascript
class CategoriesModel {
  constructor($q) {
    this.categories = [
      {"id": 0, "name": "Development"},
      {"id": 1, "name": "Design"},
      {"id": 2, "name": "Exercise"},
      {"id": 3, "name": "Humor"}
    ];
  }

  getCategories() {
    return $q.when(this.categories); // wont work!
  }
}

export default CategoriesModel;
```


```javascript
class CategoriesModel {
  constructor($q) {
    'ngInject';

    this.$q = $q;
    this.categories = [
      {"id": 0, "name": "Development"},
      {"id": 1, "name": "Design"},
      {"id": 2, "name": "Exercise"},
      {"id": 3, "name": "Humor"}
    ];
  }

  getCategories() {
    return this.$q.when(this.categories);
  }
}

export default CategoriesModel;
```

```javascript
class CategoriesModel {
  constructor($q) {}
}
```

```javascript
var CategoriesModel = (function () {
    function CategoriesModel($q) {
    }
    return CategoriesModel;
}());
```

```javascript
class CategoriesModel {
  constructor(private $q) {}
}
```

```javascript
var CategoriesModel = (function () {
    function CategoriesModel($q) {
        this.$q = $q;
    }
    return CategoriesModel;
}());
```