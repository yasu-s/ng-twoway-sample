# 概要

Angularで自作コンポーネント作成時に双方向バインディングを行うサンプルです。  
このサンプルではngModelでの双方向バインディングは行いません。（ControlValueAccessor未使用のパターン）  

@Outputのプロパティ名を`＠Inputのプロパティ名+Change`にすることで対応可能です。

# 実行環境

* Node.js 8.9.x
* TypeScript 2.9.x
* Angular 6.1.x

# 動作確認  

## 1. サンプルのダウンロード

```
git clone git@github.com:yasu-s/ng-twoway-sample.git
```

## 2. パッケージインストール  

```
cd ng-twoway-sample
npm install
```

## 3. サンプルの起動  

```
npm start
```

## 4. 実行結果  

![twoway](https://user-images.githubusercontent.com/2668146/44337941-4d70ab00-a4b7-11e8-9bdc-c14836f6ce42.gif)

# サンプルソース

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

  /** 表示リスト */
  list: number[] = [1, 2, 3, 4, 5];

  /** 選択値 */
  @Input()
  value: number = 0;

  /** 選択値変更 */
  @Output()
  valueChange = new EventEmitter<number>();

  /**
   * 選択値変更処理
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
