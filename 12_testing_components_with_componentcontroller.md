# Testing Components with $componentController

Testing directives in Angular 1.x was tricky because we had to instantiate DOM elements to test the underlying functionality. Angular introduced $componentController to allow us to test component controller without having to jump through any DOM creation hoops.