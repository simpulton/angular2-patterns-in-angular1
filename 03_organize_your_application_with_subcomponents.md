# Organize Your Application with Subcomponents

The atomic building block of an Angular 2 application is the component. 

```javascript
import {Component} from '@angular/core';
import {ItemDetailsComponent} from './item-details';

@Component({
  moduleId: module.id,
  selector: 'app-items',
  templateUrl: 'items.component.html',
  directives: [ItemDetailsComponent],
  styleUrls: ['items.component.css']
})
export class ItemsComponent {}
```

```html
<div>
  <div>
    <h2>{{ title }}</h2>
  </div>
  <div>
    {{ body }}
  </div>
</div>

<app-item-details *ngFor="let item of items" [item]="item"></app-item-details>
```

