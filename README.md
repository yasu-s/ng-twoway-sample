# Overview

It is a sample that performs bi-directional binding when creating self-made components in Angular.  
This sample does not perform bidirectional binding with ngModel. (ControlValueAccessor unused pattern)  

It is possible to correspond by setting the property name of @Output to `@Input property name + Change`.

# System requirements

* Node.js 8.9.x
* TypeScript 2.9.x
* Angular 6.1.x

# Operation check  

## 1. Download Sample

```
git clone git@github.com:yasu-s/ng-twoway-sample.git
```

## 2. Installing packages 

```
cd ng-twoway-sample
npm install
```

## 3. Launch sample application

```
npm start
```

## 4. Execution result  

![twoway](https://user-images.githubusercontent.com/2668146/44337941-4d70ab00-a4b7-11e8-9bdc-c14836f6ce42.gif)

# Sample source

## src/app/custom-list.component.ts

```typescript
import { Component, Input, Output, EventEmitter } from '@angular/core';

@Component({
  selector: 'custom-list',
  template: `
    <ul>
      <li *ngFor="let num of list"
        (click)="changeValue(num)" [style.background-color]="num === value ? 'yellow' : 'white'">
        {{ num }}
      </li>
    </ul>
  `
})
export class CustomListComponent {

  /** Display List */
  list: number[] = [1, 2, 3, 4, 5];

  /** Selected Value */
  @Input()
  value: number = 0;

  /** Selected Change Value */
  @Output()
  valueChange = new EventEmitter<number>();

  /**
   * Change Value
   * @param num
   */
  changeValue(num: number): void {
    this.value = num;
    this.valueChange.emit(num);
  }
}
```

## src/app/app.component.ts

```typescript
import { Component } from '@angular/core';

@Component({
  selector: 'app-root',
  template: `
    <h2>TwoWay Binding Sample</h2>
    <custom-list [(value)]="num"></custom-list>
    <div>num: {{ num }}</div>
  `
})
export class AppComponent {
  num: number = 2;
}
```
