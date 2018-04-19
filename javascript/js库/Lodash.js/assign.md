# Object 的多种合并方法   

`_.assign` ,  `_.merge` ,  `_.defaults` ,  `_.defaultsDeep`

```javascript
_.assign      ({}, { a: 'a' }, { a: 'bb' }) // => { a: "bb" }
_.merge       ({}, { a: 'a' }, { a: 'bb' }) // => { a: "bb" }
_.defaults    ({}, { a: 'a' }, { a: 'bb' }) // => { a: "a"  }
_.defaultsDeep({}, { a: 'a' }, { a: 'bb' }) // => { a: "a"  }
```   

**undefined**  

```javascript
_.assign      ({}, { a: 'a'  }, { a: undefined }) // => { a: undefined }
_.merge       ({}, { a: 'a'  }, { a: undefined }) // => { a: "a" }
_.defaults    ({}, { a: undefined }, { a: 'bb' }) // => { a: "bb" }
_.defaultsDeep({}, { a: undefined }, { a: 'bb' }) // => { a: "bb" }
```  
**null**  

```javascript
_.assign      ({}, { a: 'a'  }, { a: null }) // => { a: null }
_.merge       ({}, { a: 'a'  }, { a: null }) // => { a: null }
_.defaults    ({}, { a: null }, { a: 'bb' }) // => { a: null }
_.defaultsDeep({}, { a: null }, { a: 'bb' }) // => { a: null }
```  

** _.merge** and **_.defaultsDeep**  

```javascript
_.assign      ({}, {a:{a:'a'}}, {a:{b:'bb'}}) // => { "a": { "b": "bb" }}
_.merge       ({}, {a:{a:'a'}}, {a:{b:'bb'}}) // => { "a": { "a": "a","b":"bb" }}
_.defaults    ({}, {a:{a:'a'}}, {a:{b:'bb'}}) // => { "a": { "a": "a" }}
_.defaultsDeep({}, {a:{a:'a'}}, {a:{b:'bb'}}) // => { "a": { "a": "a", "b": "bb" }}
```