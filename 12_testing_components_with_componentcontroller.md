# Testing Components with $componentController

Testing directives in Angular 1.x was tricky because we had to instantiate DOM elements to test the underlying functionality. Angular introduced **$componentController** to allow us to test component controller without having to jump through any DOM creation hoops.

To see this in action, we will see a test written for our **categories** feature. I say "feature" because in the repository, the spec contains tests for the module, controller, component and template. Skimming over the highlights, we are using the **module** mock to spin up the **categories** module as well as provide a scaled down **CategoriesModel** mock. We then inject and assign **$componentController** and **CategoriesModel** to local variables that we can use later.

Jumping down to the **Controller** block, we will first create a spy on **CategoriesModel** to watch for when **getCategories** gets called. Then we will instantiate our component using **$componentController** and passing in its dependencies as the second parameter. This is where it gets slightly tricky as our lifecycle hooks do not get implicitly fired as the component is never added to the DOM but we can work around that by explicitly calling our lifecycle method.

```javascript
describe('Categories', () => {
  let component, $componentController, CategoriesModel;

  beforeEach(() => {
    window.module('categories');

    window.module($provide => {
      $provide.value('CategoriesModel', {
        getCategories: () => {
          return {
            then: () => {}
          };
        }
      });
    });
  });

  beforeEach(inject((_$componentController_, _CategoriesModel_) => {
    CategoriesModel = _CategoriesModel_;
    $componentController = _$componentController_;
  }));

  describe('Controller', () => {
    it('calls CategoriesModel.getCategories immediately', () => {
      spyOn(CategoriesModel, 'getCategories').and.callThrough();

      component = $componentController('categories', {
        CategoriesModel
      });
      component.$onInit();

      expect(CategoriesModel.getCategories).toHaveBeenCalled();
    });
  });
});
```