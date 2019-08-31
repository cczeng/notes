# 基于ng-zorro table的树结构行切换   

> 场景适用于使用键盘/按钮等动作对表进行选中状态和切换   

  

主要代码在  `nextTr()` 函数.   



```typescript
import { Component, OnInit } from '@angular/core';

export interface TreeNodeInterface {
  key: number;
  name: string;
  age: number;
  level: number;
  expand: boolean;
  address: string;
  children?: TreeNodeInterface[];
}

@Component({
  selector: 'nz-demo-table-expand-children',
  template: `
    <nz-table #expandTable [nzData]="listOfMapData">
      <thead>
        <tr>
          <th nzWidth="40%">Name</th>
          <th nzWidth="30%">Age</th>
          <th>Address</th>
        </tr>
      </thead>
      <tbody>
        <ng-container *ngFor="let data of expandTable.data">
          <ng-container *ngFor="let item of mapOfExpandedData[data.key]">
            <tr *ngIf="(item.parent && item.parent.expand) || !item.parent" [class.active]="activeItem === item" (click)="selectTr(data,item)">
              <td
                [nzIndentSize]="item.level * 20"
                [nzShowExpand]="!!item.children"
                [(nzExpand)]="item.expand"
                (nzExpandChange)="collapse(mapOfExpandedData[data.key], item, $event)"
              >
                {{ item.name }}
              </td>
              <td>{{ item.age }}</td>
              <td>{{ item.address }}</td>
            </tr>
          </ng-container>
        </ng-container>
      </tbody>
    </nz-table>
    <button nz-button nzType="primary" (click)="step(-1)">上一行</button>
    <button nz-button nzType="primary" (click)="step(1)">下一行</button>
  `,
  styles: [`
    .active {
      background: red;
    }
    button{
      margin-right: 2rem;
    }
  `]
})
export class NzDemoTableExpandChildrenComponent implements OnInit {
  activeItem = null;
  listOfMapData = [
    {
      key: '1',
      name: 'John Brown sr.',
      age: 60,
      address: 'New York No. 1 Lake Park',
      children: [
        {
          key: '11',
          name: 'John Brown',
          age: 42,
          address: 'New York No. 2 Lake Park'
        },
        {
          key: '12',
          name: 'John Brown jr.',
          age: 30,
          address: 'New York No. 3 Lake Park',
          children: [
            {
              key: '121',
              name: 'Jimmy Brown',
              age: 16,
              address: 'New York No. 3 Lake Park'
            }
          ]
        },
        {
          key: '13',
          name: 'Jim Green sr.',
          age: 72,
          address: 'London No. 1 Lake Park',
          children: [
            {
              key: '131',
              name: 'Jim Green',
              age: 42,
              address: 'London No. 2 Lake Park',
              children: [
                {
                  key: '1311',
                  name: 'Jim Green jr.',
                  age: 25,
                  address: 'London No. 3 Lake Park'
                },
                {
                  key: '1312',
                  name: 'Jimmy Green sr.',
                  age: 18,
                  address: 'London No. 4 Lake Park'
                }
              ]
            }
          ]
        }
      ]
    },
    {
      key: 2,
      name: 'Joe Black',
      age: 32,
      address: 'Sidney No. 1 Lake Park'
    }
  ];
  mapOfExpandedData: { [key: string]: TreeNodeInterface[] } = {};

  collapse(array: TreeNodeInterface[], data: TreeNodeInterface, $event: boolean): void {
    if ($event === false) {
      if (data.children) {
        data.children.forEach((listObje: any) => {
          const target = array.find((expandObj: any) => expandObj.key.replace(/,/g, '') === listObje.key);
          target.expand = false;
          this.collapse(array, target, false);
        });
      } else {
        return;
      }
    }
  }

  convertTreeToList(root: object, key: string): TreeNodeInterface[] {
    const stack: any[] = [];
    const array: any[] = [];
    const hashMap = {};
    stack.push({ ...root, level: 1, expand: false, key: key });

    while (stack.length !== 0) {
      const node = stack.pop();
      this.visitNode(node, hashMap, array);
      if (node.children) {
        for (let i = node.children.length - 1; i >= 0; i--) {
          stack.push({ ...node.children[i], level: node.level + 1, key: node.key + "," + (i + 1), expand: false, parent: node });
        }
      }
    }

    return array;
  }

  visitNode(node: TreeNodeInterface, hashMap: { [key: string]: any }, array: TreeNodeInterface[]): void {
    if (!hashMap[node.key]) {
      hashMap[node.key] = true;
      array.push(node);
    }
  }

  ngOnInit(): void {
    this.listOfMapData.forEach((item, index) => {
      this.mapOfExpandedData[item.key] = this.convertTreeToList(item, (index + 1).toString());
    });
    console.log(this.mapOfExpandedData)
  }


  selectTr(data, item) {
    console.log(data, item);
    this.activeItem = item;
  }

  step(number) {
    this.activeItem = this.nextTr(this.mapOfExpandedData, this.activeItem, number);
  }

  nextTr(mapOfExpandedData, activeItem, step: number) {

    // 无数据
    if (!Object.keys(mapOfExpandedData).length) {
      return null;
    }

    // 只有一条数据
    if (Object.keys(mapOfExpandedData).length === 1 && mapOfExpandedData['1'].length === 1) {
      activeItem = mapOfExpandedData['1'][0];
      return activeItem;
    }


    if (!activeItem) {
      activeItem = mapOfExpandedData['1'][0];
      return activeItem;
    }

    const maxLevel = Object.keys(mapOfExpandedData).length;
    const currentPath = activeItem.key.split(',').map(Number);
    const currentLevel = mapOfExpandedData[currentPath[0]];
    const currentIndex = currentLevel.findIndex(v => v.key === activeItem.key);

    // 层的最后一个
    if (currentLevel[currentLevel.length - 1] === activeItem) {

      let baseLevel = Math.abs(currentPath[0] + step) === maxLevel ? maxLevel : Math.abs(currentPath[0] + step) < maxLevel && currentPath[0] + step > 0 ? currentPath[0] + step : 1;

      if (step < 0) {
        // 处理最后一行向上, 不然直接跳转到上一层选中去了,这里还需要做一个上一层的状态开关判断，如果展开了的话则选中最后
        let nextLevels = mapOfExpandedData[baseLevel].filter(v => v.level === baseLevel);
        let nextLevel = nextLevels[nextLevels.length - 1];

        console.log(nextLevels);
        // 一层一层往下找,找到最后一项
        while (nextLevel.expand) {
          nextLevels = mapOfExpandedData[baseLevel].filter(v => v.level === nextLevel.level + 1);
          nextLevel = nextLevels[nextLevels.length - 1];
          console.log(nextLevels);
        }

        activeItem = nextLevel === activeItem ? nextLevels[nextLevels.length - 2] : nextLevel;
        return activeItem;
      }

      activeItem = mapOfExpandedData[baseLevel][0];
      return activeItem;
    }

    // 上面的逻辑处理除了一级层之外的所有最后一个，下面的逻辑处理在 2-n 层之内的切换

    // 在二级或者三级一类的层
    let currentItem: any = currentLevel[currentIndex];
    let nextIndex = currentIndex + step;
    let nextItem: any = currentLevel[nextIndex];

    while (nextItem) {
      // 当前是折叠展开的则下一行，并且下一个不是当前选中层的上层
      console.log(`
        activeItem: expand - ${activeItem.expand} level - ${activeItem.level} key - ${activeItem.key}
        nextItem  : expand - ${nextItem.expand} level - ${nextItem.level} key - ${nextItem.key}
      `);

      // 如果符合条件的则直接下一个，不符合的继续按 mapOfExpandedData 结构指向下一个
      if (
        nextItem.level > currentItem.level && currentItem.expand
        || (nextItem.children && nextItem.level === currentItem.level)
        || (nextItem.level <= currentItem.level && !currentItem.children)
        // || (!currentItem.children && currentItem.level >= nextItem.level)
        || (nextItem.level > currentItem.level && nextItem.parent.expand)
        || (nextItem.level === currentItem.level && nextItem.parent.expand)
        || (nextItem.level < currentItem.level && nextItem.key.length < currentItem.key.length)
      ) {
        activeItem = nextItem;
        return activeItem;
      }

      nextIndex = nextIndex + step;
      nextItem = currentLevel[nextIndex];
    }

    activeItem = mapOfExpandedData[currentPath[0] + 1][0]
    return activeItem;
  }

}
```

