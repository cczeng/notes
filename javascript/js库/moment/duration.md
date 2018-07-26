# duration() - 时间段  

> Moment.js中也有时间段对象。一个时间moment被定义为一个单点时刻，而时间段被定义为一个时间长度。   
> 时间段没有定义开始和结束时间，它们是一种前后的关系。一个时间段更类似于“2小时”，而不是“今天下午的2点到4点之间”。  
> 例如，一年可以被定义为366天、365天、365.25天、12个月或52周。计算两个时间之间的天或年时，使用moment#diff 比使用Durations更好。   
> —— [Moment.js中文文档系列之八时间段（Durations）
](https://itbilu.com/nodejs/npm/4JkB42p-x.html#duration-get)   

  
# 创建时间段  

```javascript
moment.duration(Number, String);
moment.duration(Number);
moment.duration(Object);
moment.duration(String);
```  

使用毫秒级的时间戳创建一个时间段:  
```js
moment.duration(2000);  // 2秒
```  
  
创建非毫秒级的单位:   

```js
moment.duration(2, 'seconds');
moment.duration(2, 'minutes');
moment.duration(2, 'hours');
moment.duration(2, 'days');
moment.duration(2, 'weeks');
moment.duration(2, 'months');
moment.duration(2, 'years');
```   

  
# humanize() - 人性化  

**有时,你想像 `moment#form` 一样友好显示,但不想创建两个时间。**   

```js
moment.duration(1, "minutes").humanize(); // a minute
moment.duration(2, "minutes").humanize(); // 2 minutes
moment.duration(24, "hours").humanize();  // a day
```   
  
默认返回值是无后缀的,如果需要带后缀可以传一个参数.   

```
moment.duration(1,"minutes").humanize(true);       // in a minute
```  

 
# milliseconds() - 毫秒  

**获取一个数字表示的毫秒数,使用 `moment.duration().milliseconds()` 返回值为 `0 ~ 999`之间的数字**   

```js
moment.duration(500).milliseconds(); // 500
moment.duration(1500).milliseconds(); // 500
moment.duration(15000).milliseconds(); // 0
```   

如果需要获取的是一个毫秒级的时间段长度,使用`moment.duration().asMilliseconds()`方法代替。   

```js
moment.duration(500).asMilliseconds(); // 500
moment.duration(1500).asMilliseconds(); // 1500
moment.duration(15000).asMilliseconds(); // 15000
```   

# seconds() - 秒   

**获取一个数字表示的秒数，使用`moment.duration().seconds()`，其返回是一个`0〜59`之间的数字。**   

```js
moment.duration(500).seconds(); // 0
moment.duration(1500).seconds(); // 1
moment.duration(15000).seconds(); // 15
```  

如果要获取的是一个秒级的时间段长度，使用moment.duration().asSeconds()方法代替。

```js
moment.duration(500).asSeconds(); // 0.5
moment.duration(1500).asSeconds(); // 1.5
moment.duration(15000).asSeconds(); // 15
```   


# minutes() - 分  

**获取其它的时间段，`moment.duration().minutes()`可以获取分钟数`（0〜59）`，`moment.duration().asMinutes()`可以获取表示分的长度。**   

```js
moment.duration().minutes();
moment.duration().asMinutes();
```  

# hours() - 小时   

**获取其它的时间段，`moment.duration().hours()`可以获取小时数`（0〜23）`，`moment.duration().asHours()`可以获取表示小时的长度。**   

```js
moment.duration().hours();
moment.duration().asHours();
```   

# days() - 天

**获取其它的时间段，`moment.duration().days()`可以获取天数`（0〜23）`，`moment.duration().asHours()`可以获取表示天的长度。**
  
```js
moment.duration().days();
moment.duration().asDays();
```   

# months() - 月   

**获取其它的时间段，`moment.duration().months()`可以获取月数`（0〜11）`，`moment.duration().asMonths()`可以获取表示月的长度。**  

**注意:** 一个月的时间长度被定义为30天   

```js
moment.duration().months();
moment.duration().asMonths();
```

# years() - 年   

**获取其它的时间段，`moment.duration().years()`可以获取年份，`moment.duration().asYears()`可以获取表示年的长度。**   

**注意：** 一年的时间长度被定义为365天   

```js
moment.duration().years();
moment.duration().asYears();
```   
