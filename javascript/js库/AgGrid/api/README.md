# AgGrid接口



- [网格属性 [Grid Properties]](https://www.ag-grid.com/javascript-grid-properties/)：用于配置[网格的属性](https://www.ag-grid.com/javascript-grid-properties/)，例如`pagination = true`。
- [网格API [Grid API]](https://www.ag-grid.com/javascript-grid-api/)：创建网格后用于与网格进行交互的方法，例如`api.getSelectedRows()`。
- [事件 [Event]](https://www.ag-grid.com/javascript-grid-events/)：网格发布的事件，用于通知应用程序状态变化，例如`rowSelected`。
- [网格回调 [[Grid Callbacks]](https://www.ag-grid.com/javascript-grid-callbacks/)：[网格](https://www.ag-grid.com/javascript-grid-callbacks/)使用回调从应用程序中检索所需的信息，例如`getRowHeight()`。
- [行节点 [Row Node]](https://www.ag-grid.com/javascript-grid-row-node/)：网格中的每一行均由行节点对象表示，该对象又具有对应用程序提供的行数据的引用。行节点包装行数据项。行节点具有用于与特定行进行交互的属性，方法和事件，例如`rowNode.setSelected(true)`。



## 属性 <span style="font-size: 14px;">[查看](https://www.ag-grid.com/javascript-grid-properties/)</span>

```typescript
let gridOption = {
   rowData: [],
    columnDefs: myColDefs,
    pagination: true,
    rowSelection: 'single',
    
    
    // events
    onRowClicekd: function(event) { console.log('A row was Clicked'); },
    onColumnResized: function(event) { console.log('A column was resized'); },
    onGridReady: function(event){ console.log('The grid is now ready'); },
    
    // calllbacks
    isScrollLag: function() { return false; } 
}
```



## api <span style="font-size: 14px;">[查看](https://www.ag-grid.com/javascript-grid-api/)</span>

```typescript
gridOptions.api.refreshView();
//调整网格中列的大小以适合可用空间
gridOptions.columnApi.sizeColumnsToFit();
```



## event <span style="font-size: 14px;">[查看](https://www.ag-grid.com/javascript-grid-events/)</span>

> 除了直接通过 `gridOptions` 对象添加时间侦听器外，还可以注册时间没类似于在本机DOM元素上注册事件

```typescript
function myRowClickedHandler(event){
    console.log('The row was clicked');
}

gridOptions.onRowClicked = myRowClickedHandler;

gridOptions.api.addEventListener('rowClicked', myRowClickedHandler);
```





# 另一个写法

- **属性**：属性定义为普通HTML属性并设置非绑定值。
- **属性**：属性定义为括在方括号中的HTML属性，并且是Angular绑定值。
- **回调**：回调实际上是一种属性，但按照惯例，它们始终是函数，并不用于中继事件信息。使用方括号将它们绑定为属性。由于回调不是事件，因此它们不会影响Angular事件周期，因此不应更改任何状态。
- **事件处理程序**：事件处理程序定义为用普通括号括起来的HTML属性，并且是Angular绑定函数。
- **API**：可通过组件访问网格API和列API。



```html
<ag-grid-angular
    #myGrid 

    [column-defs]="columnDefs"
    [show-tool-panel]="showToolPanel"

    // this is a callback
    [is-scroll-lag]="myIsScrollLagFunction"

    // these are registering event callbacks
    (cell-clicked)="onCellClicked($event)"
    (column-resized)="onColumnEvent($event)">
</ag-grid-angular>

<button (click)="myGrid.api.deselectAll()">Clear Selection</button>
```

