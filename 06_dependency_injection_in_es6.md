# Dependency Injection in ES6

Dependency injection in an Angular application using ES6 happens at the constructor level as we pass our dependencies in as parameters. This approach to dependency injection comes with a slight nuance that could trip up developers who are unfamiliar with working in a classical language.

In the example below we are injecting the **$q** service, which then becomes available for use within the constructor. Naturally, we will need to use the **$q** inside other methods within our class. Historically, in Angular 1, when we injected a dependency as a parameter to our controller or service, it was automatically available across the entire controller or service. 

```javascript
class CategoriesModel {
  constructor($q) { // $q is scope to the constructor only
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

Using a class based approach, we simply cannot reference an injected dependency outside of the constructor. In the **getCategories** method above, there is zero knowledge of **$q** because it is only scoped to the constructor function. We can make it available by assigning it to a local member within the constructor in the form of **this.$q = $q**.

> We are using **'ngInject';** to instruct **ngAnnotate** to generate strict dependency injection syntax for us.

```javascript
class CategoriesModel {
  constructor($q) {
    'ngInject'; // ng-annotate ftw

    this.$q = $q; // constructor assignment
    this.categories = [
      {"id": 0, "name": "Development"},
      {"id": 1, "name": "Design"},
      {"id": 2, "name": "Exercise"},
      {"id": 3, "name": "Humor"}
    ];
  }

  getCategories() {
    return this.$q.when(this.categories); // now we can use $q
  }
}

export default CategoriesModel;
```

Let's take a look at another example in TypeScript to illustrate constructor assignment a bit further. We have created a simple class that accepts a single argument in the constructor.

> You can try this example out http://www.typescriptlang.org/play/index.html

```javascript
class CategoriesModel {
  constructor($q) {}
}
```

TypeScript generates the following ES5 output. Notice that **$q** is treated as just an ordinary parameter.

```javascript
var CategoriesModel = (function () {
    function CategoriesModel($q) {
    }
    return CategoriesModel;
}());
```

TypeScript allows us to do automatic constructor assignment which means that whenever we add an access modifier to the parameter, it is implied that that parameter should become a member of the class. Therefore, TypeScript will automatically assign it to a local member within the constructor.

```javascript
class CategoriesModel {
  constructor(private $q) {}
}
```

Notice that we added **private** to the parameter and the ES5 is now assigning it to a member of the class below.

```javascript
var CategoriesModel = (function () {
    function CategoriesModel($q) {
        this.$q = $q;
    }
    return CategoriesModel;
}());
```

For some people this is a "well... duh!" point to make. But assembling code around classes is an entirely new concept, even for experienced JavaScript developers.