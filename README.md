# Angular 1 in an Angular 2 Style
It was an alarming experience for most Angular 1 developers the first time they saw Angular 2. In fact, pretty much **everything** changed. We were faced with learning a new language, new syntax, new tooling, etc and it was overwhelming. The 5 minute quick start was more like a 25 minute quick start that included a 20 minute scenic tour into a ton of new stuff seemingly unrelated to Angular.



Angular 2 did not look anything like the Angular 1 that we were used to writing. For many of us, this meant going back to the drawing board and learning Angular all over again. Then something strange started to happen. The way that we approached Angular 1 applications began to change. Tiny subtle changes. We began to prefer components as the primary mechanism for composing our apps. We started to use ES6 to write our Angular 1 applications. We began to refactor our controllers to be as thin as possible and in some cases, our components didn't even **need** controllers!

## The Ethos of Angular 2



![](http://onehungrymind-45fd.kxcdn.com/books/angular2-breakdown.png)

Eventually, I started to make the distinction between "the architecture" and "the implementation" when talking about Angular 2. 

> ####Ethos
> the distinctive character, spirit, and attitudes of a people, culture, era, etc

The best practices that emerged from Angular 1 had been made the default in Angular 2. Major portions of the framework were left on the cutting floor as the Angular team decided in favor of letting vanilla JavaScript and HTML do the hard work. We had twice the power with half the framework.

And the best part of this brave new world is that a lot of the architectural principles have been back-ported into Angular 1 and we can start using them right now. Writing an Angular 1 application in an Angular 2 style invariably gets us **a lot** further down the road with the migration talk but we should not limit ourselves to just context. Writing an Angular 1 application in an Angular 2 means that we are writing better Angular 1 apps. Much better Angular 1 apps. 