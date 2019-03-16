# 防止浏览器自动填充input密码

当你设置`input` 类型为 `password` 的时候，浏览器会默认填充上账户密码，导致出现不想要的结果.   

措施: 默认类型设置为`text`,当输入框成为焦点的时候改为`password`   

```html
<input [type]="pwdType" placeholder="" (focus)="pwdType = 'password'" [(ngModel)]="pwd">
```

只需在组件里声明一个变量,默认为 `text` 即可.

```javascript
pwdType = 'text';
```