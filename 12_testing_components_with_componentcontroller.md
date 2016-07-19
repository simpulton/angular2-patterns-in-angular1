# Testing Components with $componentController

Testing directives in Angular 1.x was tricky because we had to instantiate DOM elements to test the underlying functionality. Angular introduced **$componentController** to allow us to test component controller without having to jump through any DOM creation hoops.

To see this in action, we will see a test written for our **categories** feature. I say "feature" because in the repository, the spec contains tests for the module, controller, component and template. Skimming over the highlights, we are using the **module** mock to spin up the **categories** module as well as provide a scaled down **CategoriesModel** mock. We then inject and assign **$componentController** and **CategoriesModel** to local variables that we can use later.

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