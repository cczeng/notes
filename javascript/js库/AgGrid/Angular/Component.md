# Angular Componet && agGrid

> 如果你正在使用网格的Angular组件版本，则可以使用Angular组件自定义网格的内部。例如，您可以使用组件来自定义单元格的内容。



为了使ag-Grid能够使用组件，需要在**顶层模块**中提供它们：

```typescript
@NgModule({
imports: [
    BrowserModule,
    FormsModule,
    RouterModule.forRoot(appRoutes),
    AgGridModule.withComponents(
        [
            SquareComponent,
            CubeComponent,
            // ...other components
```



然后，可以将这些组件用作编辑器，渲染器或者过滤器。例如，要将Angular Component用作Cell Renderer，可以执行以下操作： 

```typescript
let colDefs = [
    {
        headerName: "Square Component", # 表头标题
        field: "value",
        cellRendererFramework: SquareComponent, # 这里把组件给进去
        editable:true,
        colId: "square",
        width: 175
    },
    ...other column definitions
]
```

请参阅有关[单元格渲染器](https://www.ag-grid.com/javascript-grid-cell-rendering-components/#ng2CellRendering)，[单元格编辑器](https://www.ag-grid.com/javascript-grid-cell-editing/#ng2CellEditing)和 [过滤](https://www.ag-grid.com/javascript-grid-filtering/#ng2Filtering)[器](https://www.ag-grid.com/javascript-grid-cell-rendering-components/#ng2CellRendering)的相关部分， 以在ag-Grid中配置和使用Angular组件。



## 父子组件通讯

有多种方法可以管理Angular中的组件通信（共享服务，局部变量等），但是您通常需要一种简单的方法来让“父”组件知道“子”组件上发生了某些事情。在这种情况下，最简单的方法是使用`gridOptions.context`持有对父项的引用，子项随后可以访问该引用。

```typescript
// in the parent component - the component that hosts ag-grid-angular and specifies which angular components to use in the grid
constructor() {
    this.gridOptions = <GridOptions>{
        context: {
            componentParent: this
        }
    };
    this.gridOptions.rowData = this.createRowData();
    this.gridOptions.columnDefs = this.createColumnDefs();
}

// in the child component - the angular components created dynamically in the grid
// the parent component can then be accessed as follows:
this.params.context.componentParent
```

简单理解为： 

- 在父组件里面传递 `gridOptions` 给ag-grid-angular
- `gridOptions` 配置里面放入当前父组件的索引，以及加入其它方法
- 在子组件里面实现`ICellRendererAngularComp`接口的 `agInit` 方法
- 在子组件里面使用 `params.context.componentParent` 来获取父组件的引用



## 实例



### 父组件

```typescript
import {Component} from "@angular/core";

import {GridOptions} from "ag-grid-community";
import {SquareComponent} from "./square.component";
import {ParamsComponent} from "./params.component";
import {CubeComponent} from "./cube.component";
import {CurrencyComponent} from "./currency.component";
import {ChildMessageComponent} from "./child-message.component";

@Component({
    selector: 'ag-from-component',
    templateUrl: './dynamic.component.html'
})
export class DynamicComponent {
    public gridOptions: GridOptions;

    constructor() {
        this.gridOptions = <GridOptions>{
            rowData: DynamicComponent.createRowData(),
            columnDefs: DynamicComponent.createColumnDefs(),
            context: {
                componentParent: this
            }
        };
    }

    // noinspection JSMethodCanBeStatic
    public methodFromParent(cell) {
        alert(`"Parent Component Method from ${cell}!`);
    }

    private static createColumnDefs() {
        return [
            {headerName: "Row", field: "row", width: 100},
            {
                headerName: "Square",
                field: "value",
                cellRendererFramework: SquareComponent,
                editable: true,
                colId: "square",
                width: 100
            },
            {
                headerName: "Cube",
                field: "value",
                cellRendererFramework: CubeComponent,
                colId: "cube",
                width: 100
            },
            {
                headerName: "Row Params",
                field: "row",
                cellRendererFramework: ParamsComponent,
                colId: "params",
                width: 215
            },
            {
                headerName: "Currency (Pipe)",
                field: "currency",
                cellRendererFramework: CurrencyComponent,
                colId: "currency",
                width: 135
            },
            {
                headerName: "Child/Parent",
                field: "value",
                cellRendererFramework: ChildMessageComponent,
                colId: "params",
                width: 120
            }
        ];
    }

    public refreshEvenRowsCurrencyData() {
        this.gridOptions.api.forEachNode(rowNode => {
            if (rowNode.data.value % 2 === 0) {
                rowNode.setDataValue('currency', rowNode.data.value + Number(Math.random().toFixed(2)))
            }
        });

        this.gridOptions.api.refreshCells({
            columns: ['currency']
        })
    }

    private static createRowData() {
        let rowData: any[] = [];

        for (let i = 0; i < 15; i++) {
            rowData.push({
                row: "Row " + i,
                value: i,
                currency: i + Number(Math.random().toFixed(2))
            });
        }

        return rowData;
    }
}
```



### 子组件

```typescript
import {Component} from "@angular/core";
import {ICellRendererAngularComp} from "ag-grid-angular/main";

@Component({
    selector: 'child-cell',
    template: `<span><button style="height: 20px" (click)="invokeParentMethod()" class="btn btn-info">Invoke Parent</button></span>`,
    styles: [
        `.btn {
            line-height: 0.5
        }`
    ]
})
export class ChildMessageComponent implements ICellRendererAngularComp {
    public params: any;

    agInit(params: any): void {
        this.params = params;
    }

    public invokeParentMethod() {
        this.params.context.componentParent.methodFromParent(`Row: ${this.params.node.rowIndex}, Col: ${this.params.colDef.headerName}`)
    }

    refresh(): boolean {
        return false;
    }
}
```





