# 返回异步数据   
 

假设这是某个`service` 中的搜索方法:  

```javascript
search (term: string) {
  var search = new URLSearchParams();
  search.set('action', 'opensearch');
  search.set('search', term);
  search.set('format', 'json');
  return this.jsonp
              .get('http://xxx.com/test.php?callback=JSONP_CALLBACK', { search })
              .map((response) => response.json()[1]);
}
```   

在 `component` 中:    

```javascript
@Component({
  selector: 'my-app',
  template: `
    <div>
      <h2>Search</h2>
      <input type="text" [formControl]="term"/>
      <ul>
        <li *ngFor="let item of items | async">{{item}}</li>   // 这里使用了异步管道处理异步数据
      </ul>
    </div>
  `
})
export class AppComponent {
  items: Observable<Array<string>>;
  term = new FormControl();
  constructor(private myService: MyService) {
    this.items = this.term.valueChanges
                 .debounceTime(400)         // 去抖动
                 .distinctUntilChanged()    // 两次数据一样的话不触发
                 .switchMap(term => this.wikipediaService.search(term));   // 这个方法会丢弃之前所有的搜索，只留下最后一个
  }
}
```