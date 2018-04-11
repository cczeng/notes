#  indexOf method in an Object array

从一个对象数组中查找出一个对象的方法。

另外请参见 [stack overflow](https://stackoverflow.com/questions/8668174/indexof-method-in-an-object-array)

1. [Map().indexOf()](#index)
2. [findIndex()](#findindex)
3. [indexOf(arr.filter())](filter)



<span id="index"></span>

### map().indexOf()

```javascript
const index = users.map(user => user.id ).indexOf(myUser.id);
```

这种方法在数组短的情况下性能较好.

<span id="findindex"></span>

### findIndex

```javascript
const index = users.findIndex(user => user.id === myUser.id);
```

这种综合情况下性能都不错



<span id="filter"><span>

### indexOf(arr.filter())

```javascript
const index = users.indexOf(users.filter(user => user.id === myUser.id)[0]);
```







