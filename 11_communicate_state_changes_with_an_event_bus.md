# Communicate State Changes with an Event Bus

Angular 2 uses observables to communicate state change across the application. We do not have the same luxury of leverage observables at the framework level in Angular 1.x, but we can approximate the pattern by creating an event bus to communicate state change.