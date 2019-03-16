# [Reactive Forms - 响应式表单](https://angular.io/guide/reactive-forms)

**响应式表单**提供了一种模型驱动的方式来处理表单输入，其中的值会随时间而变化。

[在线例子](https://stackblitz.com/angular/njgmxgqljap)



> 响应式表单使用显示的、不可变的方式，管理表单在特定的时间点上的状态。对表单状态的每一次变更都会返回一个新状态，这样可以在变化时维护模型的整体性。响应式表单是围绕Observable的流构建的，表单的输入和值都是通过这些输入值组成的流来提供的，同事，也赋予你对数据进行同步访问的能力。这种方式允许你的模板利用这些表单的“状态变更流”，而不必依赖它们。

响应式表单与模板驱动的表单有着显著的不同点。响应式表单通过对数据模型的同步访问提供了更多的可预测性，使用 Observable 的操作符提供了不可变性，并且通过 Observable 流提供了变化追踪功能。 如果你更喜欢在模板中直接访问数据，那么模板驱动的表单会显得更明确，因为它们依赖嵌入到模板中的指令，并借助可变数据来异步跟踪变化。参见[附录](https://angular.cn/guide/reactive-forms#appendix)来了解这两种范式之间的详细比较。

 ## 注册 ReactiveFormsModule

要使用响应式表单就必须导入相应模块：  

```js
import { ReactiveFormsModule } form '@angular/forms';

@NgModule({
    imports: [
        // other imports
        ReactiveFormsModule
    ]
})

export class AppModule{}
```

## 导入并创建一个新的表单控件

当使用响应式表单时，`FormControl` 是最基本的构造块。要注册单个的表单控件，请在组件中导入 `FormControl` 类，并创建一个 `FormControl` 的新实例，把它保存在类的某个属性中。

```js
import { Component } from '@angular/core';
import { FormControl } from '@angular/forms';

@Component({
  selector: 'app-name-editor',
  templateUrl: './name-editor.component.html',
  styleUrls: ['./name-editor.component.css']
})
export class NameEditorComponent {
  name = new FormControl('');
}
```

`FormControl` 的构造函数可以设置初始值，这个例子中它是空字符串。通过在你的组件类中创建这些控件，你可以直接对表单控件的状态进行监听、修改和校验。

## 在模板中注册该控件

在组件类中创建了控件之后，你还要把它和模板中的一个表单控件关联起来。修改模板，为表单控件添加 `formControl`绑定，`formControl` 是由 `ReactiveFormsModule` 中的 `FormControlDirective` 提供的。

```html
<label>
  Name:
  <input type="text" [formControl]="name">
</label>
```





## 管理控件的值

响应式表单让你可以访问表单控件刺客的状态和值。你可以通过组件类或组件模板来操作其当前状态和值。



### 显示控件的值

每个`FormControl`都会通过一个名叫`valueChanges`的Observable型属性提供它当前值。可以在模板中使用`AsyncPipe`来监听模板中表单值的变化，或者在组件类中使用`subscribe()`方法来监听。`value`属性也可以提供一个当前值的一个快照。



```html
<p>
    Value: {{name.value}}
</p>
```



### 替换表单控件的值

响应式表单还有一些方法可以用编程的方式修改控件的值，它让你可以灵活的修改控件的值而不需要借助用户交互。`FormControl` 提供了一个 `setValue()` 方法，它会修改这个表单控件的值，并且验证与控件结构相对应的值的结构。比如，当从后端 API 或服务接收到了表单数据时，可以通过 `setValue()` 方法来把原来的值替换为新的值。

```js
updateName(){
    this.name.setValue('Nancy');
}
```

```html
<p>
    <button (click)="updateName()">
        Update Name
    </button>
</p>
```

> *注意*：在这个例子中，你只使用单个控件，但是当调用 `FormGroup` 或 `FormArray` 的 `setValue()` 方法时，传入的值就必须匹配控件组或控件数组的结构才行。

## 把表单控件分组

正如 `FormControl` 的实例能让你控制单个输入框所对应的控件，`FormGroup` 可以跟踪一组 `FormControl` 实例（比如一个表单）的表单状态。当创建 `FormGroup` 时，其中的每个控件都会根据其名字进行跟踪。下列例子展示了如何管理单个控件组中的多个 `FormControl` 实例。

```js
import { FormGroup, FormControl } from '@angular/forms';
```

### 创建FormGroup

在组件类中创建一个名叫 `profileForm` 的属性，并设置为 `FormGroup` 的一个新实例。要初始化这个 `FormGroup`，请为构造函数提供一个由控件组成的对象，对象中的每个名字都要和表单控件的名字一一对应。

 ```js
import { Component } from '@angular/core';
import { FormGroup, FormControl } from '@angular/forms';

@Component({
  selector: 'app-profile-editor',
  templateUrl: './profile-editor.component.html',
  styleUrls: ['./profile-editor.component.css']
})
export class ProfileEditorComponent {
  profileForm = new FormGroup({
    firstName: new FormControl(''),
    lastName: new FormControl(''),
  });
}
 ```



### 关联 FormGroup 的模型和视图

```html
<form [formGroup]="profileForm">
  
  <label>
    First Name:
    <input type="text" formControlName="firstName">
  </label>

  <label>
    Last Name:
    <input type="text" formControlName="lastName">
  </label>

</form>
```

注意，就像 `FormGroup` 所包含的那控件一样，*profileForm* 这个 `FormGroup` 也通过 `FormGroup` 指令绑定到了 `form` 元素，在该模型和表单中的输入框之间创建了一个通讯层。 由 `FormControlName` 指令提供的 `formControlName` 属性把每个输入框和 `FormGroup` 中定义的表单控件绑定起来。这些表单控件会和相应的元素通讯，它们还把更改传递给 `FormGroup`，这个 `FormGroup` 是模型值的真正源头。



### 保存表单数据

`ProfileEditor` 组件从用户那里获得输入，但在真实的场景中，你可能想要先捕获表单的值，等将来在组件外部进行处理。 `FormGroup` 指令会监听 `form` 元素发出的 `submit` 事件，并发出一个 `ngSubmit` 事件，让你可以绑定一个回调函数。

把 `onSubmit()` 回调方法添加为 `form` 标签上的 `ngSubmit` 事件监听器。



```html
<form [formGroup]="profileForm" (ngSubmit)="onSubmit()">
```

```js
onSubmit() {
  // TODO: Use EventEmitter with form value
  console.warn(this.profileForm.value);
}
```

往表单的底部添加一个`button`，用于触发表单提交。

```html
<button type="submit" [disabled]="!profileForm.valid">Submit</button>
```



## 嵌套的表单组

如果要构建复杂的表单，如果能在更小的分区中管理不同类别的信息就会更容易一些，而有些信息分组可能会自然的汇入另一个更大的组中。使用嵌套的 `FormGroup` 可以让你把大型表单组织成一些稍小的、易管理的分组。



### 创建嵌套的分组

“地址”就是可以把信息进行分组的绝佳范例。`FormGroup` 可以同时接纳 `FormControl` 和 `FormGroup` 作为子控件。这使得那些比较复杂的表单模型可以更易于维护、更有逻辑性。要想在 `profileForm` 中创建一个嵌套的分组，请添加一个内嵌的名叫 `address` 的 `FormGroup`。

`Profile-editor.component.ts:`

```ts
import { Component } from '@angular/core';
import { FormGroup, FormControl } from '@angular/forms';

@Component({
  selector: 'app-profile-editor',
  templateUrl: './profile-editor.component.html',
  styleUrls: ['./profile-editor.component.css']
})
export class ProfileEditorComponent {
  profileForm = new FormGroup({
    firstName: new FormControl(''),
    lastName: new FormControl(''),
    address: new FormGroup({
      street: new FormControl(''),
      city: new FormControl(''),
      state: new FormControl(''),
      zip: new FormControl('')
    })
  });
}
```

来自内嵌控件组的状态和值的变更将会冒泡到它的父控件组，以维护整体模型的一致性。



### 在模板中分组内嵌的表单

```html
<div formGroupName="address">
  <h3>Address</h3>

  <label>
    Street:
    <input type="text" formControlName="street">
  </label>

  <label>
    City:
    <input type="text" formControlName="city">
  </label>
  
  <label>
    State:
    <input type="text" formControlName="state">
  </label>

  <label>
    Zip Code:
    <input type="text" formControlName="zip">
  </label>
</div>
```



## 部分模型更新

当修改包含多个控件的 `FormGroup` 的值时，你可能只希望更新模型中的一部分，而不是完全替换掉。



### 修补(Patch)模型值

对单个控件，你会使用 `setValue()` 方法来该控件设置新值。但当应用到 `FormGroup` 并打算整体设置该控件的值时，`setValue()` 方法会受到这个 `FormGroup` 结构的很多约束。`patchValue()` 方法就宽松多了，它只会替换表单模型中修改过的那些属性，因为你只想提供部分修改。`setValue()` 中严格的检查可以帮你捕获复杂表单嵌套时可能出现的错误，而 `patchValue()` 将会默默地走向失败。



```ts
updateProfile() {
  this.profileForm.patchValue({
    firstName: 'Nancy',
    address: {
      street: '123 Drew Street'
    }
  });
}
```

```html
<p>
  <button (click)="updateProfile()">Update Profile</button>
</p>
```



## 使用 FormBuilder 来生成表单控件

当需要与多个表单打交道时，手动创建多个表单控件实例会非常繁琐。`FormBuilder` 服务提供了一些便捷方法来生成表单控件。`FormBuilder` 在幕后也使用同样的方式来创建和返回这些实例，只是用起来更简单。 下面的小节中会重构 `ProfileEditor` 组件，用 `FormBuilder` 来代替手工创建这些 `FormControl` 和 `FormGroup`。



### 导入 FormBuilder 类

```js
import { FormBuilder } from '@angular/forms';
```

### 注入 FormBuilder 服务

```js
constructor(private fb: FormBuilder) {}
```

### 生成表单控件

`FormBuilder` 服务有三个方法：`control()`、`group()` 和 `array()`。这些方法都是工厂方法，用于在组件类中分别生成 `FormControl`、`FormGroup` 和 `FormArray`。

```ts
import { Component } from '@angular/core';
import { FormBuilder } from '@angular/forms';

@Component({
  selector: 'app-profile-editor',
  templateUrl: './profile-editor.component.html',
  styleUrls: ['./profile-editor.component.css']
})
export class ProfileEditorComponent {
  profileForm = this.fb.group({
    firstName: [''],
    lastName: [''],
    address: this.fb.group({
      street: [''],
      city: [''],
      state: [''],
      zip: ['']
    }),
  });

  constructor(private fb: FormBuilder) { }
}
```





## 简单表单验证

当通过表单接收用户输入时，表单验证是必要的。



### 导入验证器函数

响应式表单包含了一组开箱即用的常用验证器函数。这些函数接收一个控件，用以验证并根据验证结果返回一个错误对象或空值。



```ts
import { Validators } from '@angular/forms';
```

### 把字段设为必填

在 `ProfileEditor` 组件中，把静态方法 `Validators.required` 设置为 `firstName` 控件值数组中的第二项。

```ts
profileForm = this.fb.group({
  firstName: ['', Validators.required],
  lastName: [''],
  address: this.fb.group({
    street: [''],
    city: [''],
    state: [''],
    zip: ['']
  }),
});
```

HTML5 有一组内置的属性，用来进行原生验证，包括 `required`、`minlength`、`maxlength` 等。虽然是*可选的*，不过你也可以在表单的输入元素上把它们添加为附加属性来使用它们。这里我们把 `required` 属性添加到 `firstName` 输入元素上。

```html
<input type="text" formControlName="firstName" required>
```



### 显示表单状态

你可以通过该 `FormGroup` 实例的 `status` 属性来访问其当前状态。

```html
<p>
  Form Status: {{ profileForm.status }}
</p>
```



## 使用表单数组管理动态控件

`FormArray` 是 `FormGroup` 之外的另一个选择，用于管理任意数量的匿名控件。像 `FormGroup` 实例一样，你也可以往 `FormArray` 中动态插入和移除控件，并且 `FormArray` 实例的值和验证状态也是根据它的子控件计算得来的。 不过，你不需要为每个控件定义一个名字作为 key，因此，如果你事先不知道子控件的数量，这就是一个很好的选择。下面的例子展示了如何在 `ProfileEditor` 中管理一组*绰号*（aliases）。



### 导入 FormArray

```ts
import { FormArray } from '@angular/forms';
```

### 定义 FormArray

你可以通过把一组（从零项到多项）控件定义在一个数组中来初始化一个 `FormArray`。为 `profileForm` 添加一个 `aliases` 属性，把它定义为 `FormArray` 类型。

```ts
profileForm = this.fb.group({
  firstName: ['', Validators.required],
  lastName: [''],
  address: this.fb.group({
    street: [''],
    city: [''],
    state: [''],
    zip: ['']
  }),
  aliases: this.fb.array([
    this.fb.control('')
  ])
});
```

### 访问 FromArray 控件

因为 `FormArray` 表示的是数组中具有未知数量的控件，因此通过 getter 来访问控件比较便捷，也容易复用。使用 `getter` 语法来创建一个名为 `aliases` 的类属性，以便从父控件 `FormGroup` 中接收绰号的 `FormArray` 控件。



```ts
get aliases(){
    return this.profileForm.get('aliases') as FormArray;
}
```

定义一个方法来把一个绰号控件动态插入到绰号 `FormArray` 中。用 `FormArray.push()` 方法把该控件添加为数组中的新条目。

```js
addAlias(){
    this.aliases.push(this.fb.control(''));
}
```

### 在模板中显示表单数组

在模型中定义了 `aliases` 的 `FormArray` 之后，你必须把它加入到模板中供用户输入。和 `FormGroupNameDirective` 提供的 `formGroupName` 一样，`FormArrayNameDirective` 也使用 `formArrayName` 在这个 `FormArray` 和模板之间建立绑定。

```html
<div formArrayName="aliases">
  <h3>Aliases</h3> <button (click)="addAlias()">Add Alias</button>

  <div *ngFor="let address of aliases.controls; let i=index">
    <!-- The repeated alias template -->
    <label>
      Alias:
      <input type="text" [formControlName]="i">
    </label>
  </div>
</div>
```

`*ngFor` 指令对 `aliases` `FormArray` 提供的每个 `FormControl` 进行迭代。因为 `FormArray` 中的元素是匿名的，所以你要把*索引号*赋值给 `i` 变量，并且把它传给每个控件的 `formControlName` 输入属性。





# 附录



## 响应式表单API

| 类                | 说明                                                         |
| ----------------- | ------------------------------------------------------------ |
| `AbstractControl` | 所有三种表单控件类（`FormControl`、`FormGroup` 和 `FormArray`）的抽象基类。它提供了一些公共的行为和属性。 |
| `FormControl`     | 管理单体表单控件的值和有效性状态。它对应于 HTML 的表单控件，比如 `<input>` 或 `<select>`。 |
| `FormGroup`       | 管理一组 `AbstractControl` 实例的值和有效性状态。该组的属性中包括了它的子控件。组件中的顶级表单就是 `FormGroup`。 |
| `FormArray`       | 管理一些 `AbstractControl` 实例数组的值和有效性状态。        |
| `FormBuilder`     | 一个可注入的服务，提供一些用于提供创建控件实例的工厂方法。   |



**Directives：**

| 指令                   | 说明                                                         |
| ---------------------- | ------------------------------------------------------------ |
| `FormControlDirective` | 把一个独立的 `FormControl` 实例绑定到表单控件元素。          |
| `FormControlName`      | 把一个现有 `FormGroup` 中的 `FormControl` 根据名字绑定到表单控件元素。 |
| `FormGroupDirective`   | 把一个现有的 `FormGroup` 绑定到 DOM 元素。                   |
| `FormGroupName`        | 把一个内嵌的 `FormGroup` 绑定到一个 DOM 元素。               |
| `FormArrayName`        | 把一个内嵌的 `FormArray` 绑定到一个 DOM 元素。               |



##  与模板驱动表单的对比

[模板驱动表单](https://angular.cn/guide/forms)一章中介绍的*模板驱动*表单是一种截然不同的方式。

- 你把 HTML 表单控件（比如 `<input>` 和 `<select>`）放进组件模板中，并且使用 `ngModel` 等指令把它们绑定到组件中的*数据模型*的属性。
- 你不能创建 Angular 表单控件对象，Angular 指令会根据你提供的数据绑定信息替你创建它们。
- 你不能推拉数据值。Angular 会用 `ngModel` 替你处理它们。当用户进行修改时，Angular 会更新这个可变的*数据模型*。

