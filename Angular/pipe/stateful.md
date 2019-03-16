# [Stateful Pipes 有状态管道](https://stackoverflow.com/questions/34456430/ngfor-doesnt-update-data-with-pipe-in-angular2/34497504)

> pipe 默认情况下是无状态的,也就是输入数组变化并不会再次触发管道.   

然而在有些情况下,我们需要的是有状态管道.   

## 0x01:  
这种方法是最简单的,只需要在管道中加入 `pure:false` 即可
```javascript
import { Pipe, PipeTransform } from '@angular/core';

@Pipe({
  name: 'usersInGroup',
  pure: false
})
```  


## 0x02   

使用`changeDetection` 进行变更检测,但是只检测 `onPush`  

参见[Plunker](http://plnkr.co/edit/Q2PYqpXZg0HcsjAwuGGk?p=preview)   


```javascript
import {Component, ChangeDetectionStrategy, EventEmitter } from 'angular2/core';
import {SortByNamePipe} from './sort_by_name_pipe';

@Component({
  selector: 'hello-world',
  templateUrl: 'src/hello_world.html',
  pipes: [SortByNamePipe],
  events: ['studentChange'],
  changeDetection: ChangeDetectionStrategy.OnPush,
})
export class HelloWorld {
  students : string[];
  studentChange = new EventEmitter();
  constructor() {
    console.clear();
    this.students = [{
      name: 'Son'
    },{
      name: 'David'
    },{
      name: 'Edward'
    }]
  }
  addNewStudent(studentName: string){
    this.students.push({
      name: studentName
    });
    //console.log(this.students);
  }
}
```