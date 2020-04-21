# agGrid in Angular



## 安装

由于`agGrid`使用的是`scss`所以建议项目新建为`scss`  

```npm
npm install -g @angular/cli
ng new my-app --style=scss --routing=true
cd my-app
ng serve
```



接下来在项目中引入： 

```npm
npm install --save ag-grid-community ag-grid-angular
npm install
```

`app.module.ts`: 

```typescript
import { BrowserModule } from '@angular/platform-browser';
import { NgModule } from '@angular/core';

import { AppComponent } from './app.component';
import { AgGridModule } from 'ag-grid-angular';

@NgModule({
  declarations: [AppComponent],
  imports: [
    BrowserModule,
    AgGridModule.withComponents([])
  ],
  providers: [],
  bootstrap: [AppComponent]
})
export class AppModule {}
```

`style.scss` :

```scss
@import "../node_modules/ag-grid-community/src/styles/ag-grid.scss";
@import "../node_modules/ag-grid-community/src/styles/ag-theme-alpine/sass/ag-theme-alpine-mixin.scss";

.ag-theme-alpine {
    @include ag-theme-alpine();
}
```





## 使用



组件： 

```html
<ag-grid-angular
    style="width: 500px; height: 500px;"
    class="ag-theme-alpine"
    [rowData]="rowData"
    [columnDefs]="columnDefs"
    >
</ag-grid-angular>
```

```typescript
import { Component } from '@angular/core';

@Component({
    selector: 'app-root',
    templateUrl: './app.component.html',
    styleUrls: ['./app.component.scss']
})
export class AppComponent {
    title = 'app';

    columnDefs = [
        {headerName: 'Make', field: 'make' },
        {headerName: 'Model', field: 'model' },
        {headerName: 'Price', field: 'price'}
    ];

    rowData = [
        { make: 'Toyota', model: 'Celica', price: 35000 },
        { make: 'Ford', model: 'Mondeo', price: 32000 },
        { make: 'Porsche', model: 'Boxter', price: 72000 }
    ];
}
```



### 简单功能



**添加排序和搜索过滤**

```typescript
columnDefs = [
    {headerName: 'Make', field: 'make', sortable: true, filter: true},
    {headerName: 'Model', field: 'model', sortable: true, filter: true},
    {headerName: 'Price', field: 'price', sortable: true, filter: true}
];
```



**异步** 

```typescript
ngOnInit() {
    this.rowData = this.http.get('https://raw.githubusercontent.com/ag-grid/ag-grid/master/grid-packages/ag-grid-docs/src/sample-data/rowData.json');
}
```

```html
<ag-grid-angular
    style="width: 500px; height: 500px;"
    class="ag-theme-alpine"
    [rowData]="rowData | async"  # 使用 async 管道即可
    [columnDefs]="columnDefs"
    >
</ag-grid-angular>
```



**启用选择**

```typescript
columnDefs = [
    {headerName: 'make', field: 'make', sortable: true, filter: true, checkboxSelection: true }, // 添加 CheckBoxSelection
    {headerName: 'model', field: 'model', sortable: true, filter: true },
    {headerName: 'price', field: 'price', sortable: true, filter: true }
];
```

```html
<ag-grid-angular
    #agGrid
    style="width: 500px; height: 500px;"
    class="ag-theme-alpine"
    [rowData]="rowData | async"
    [columnDefs]="columnDefs"
    rowSelection="multiple" #添加
    >
</ag-grid-angular>
```

获取被选中的项： 

```typescript
getSelectedRows() {
    const selectedNodes = this.agGrid.api.getSelectedNodes();
    const selectedData = selectedNodes.map( node => node.data ); // 此处已经拿到数据
}
```



### 复杂功能

可参考[在线例子](https://www.ag-grid.com/angular-markup/) 直接拉最底下的例子点击右上角 Plunker



