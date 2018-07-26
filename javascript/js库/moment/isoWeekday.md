# isoWeekday 获取周  

> 获取当前是周几,获取本周x日期  

## 获取当前为周几  

```javascript
moment().isoWeekday(); // Number 1 - 7
// 4
```

## 获取当前周x的日期  

```javascript
moment().isoWeekday(6);
// 输出本周六的日期

moment().isoWeekday("Sunday");  // 使用当前语言环境
```   
## 获取本周之外的时间  

**只需要给出一个超出时间范围的即可**  

`.weekday()` 和 `isoWeekday()` 有一点区别,在于传入的参数: `weekday()` 传入时间为 `0 - 6` 而后者为 `1 - 7`

```javascript
// when Monday is the first day of the week
moment().weekday(-7); // last Monday
moment().weekday(7); // next Monday
// when Sunday is the first day of the week
moment().weekday(-7); // last Sunday
moment().weekday(7); // next Sunday
```