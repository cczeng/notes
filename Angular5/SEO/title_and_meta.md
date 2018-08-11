# Title and meta  -- 动态修改 title meta 属性   

> 引自 [Angular Universal](https://blog.angular-university.io/angular-universal/) 代码片段  


在需要做SEO的页面上为每个不同的页面进行添加:

```js
@Component({
    selector: 'course',
    templateUrl: './course.component.html',
    styleUrls: ['./course.component.css']
})
export class CourseComponent implements OnInit {

    course: Course;

    constructor(private title: Title,
                private meta: Meta) {}

    ngOnInit() {
        this.course = this.route.snapshot.data['course'];
        ....
        // SEO metadata
        this.title.setTitle(this.course.description);
        this.meta.addTag({name: 'description', content: this.course.longDescription});
    }
}
```



在首页上添加,因为目前百度不能抓取javascript,所以组件的内容也抓取不到。

`index.html`

```html
<title>关键词1_关键词2_关键词3 – 网站的品牌</title> // 常用格式
<title>关键词1_关键词2_关键词3 –广告语</title> // 升级版,竞争较强的词语往往用广告语来吸引点击
<title>品牌词_关键词1_关键词2–广告语</title> // 升级版,品牌效应


<meta name="keywords" content="关键词1,关键词2,关键词3"/>  // 注意关键词与关键词之间用英文(半角)的逗号隔开，目标关键词设置在3个左右最佳。
<meta name="keywords" content="深圳SEO,深圳网站优化,深圳营销型网站"/>   // 例子

<meta name="description" content="描述,对网站的描述"/>      // 对网站的描述
<meta name="description" content="关键词1关键词2关键词3是我们的主要经营产品，所以你需要1关键词2关键词3请联系（公司名称+联系电话）"/>      // 例子

```  

上面可参考另一篇文章讲到的 ``