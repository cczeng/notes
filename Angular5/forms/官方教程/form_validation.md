# [Form Validatio - 表单验证](https://angular.io/guide/form-validation)

> 通过验证用户输入的准确性和完整性,来增强整体数据质量  

[在线例子](https://stackblitz.com/angular/gkpeayjxelp?file=src%2Fapp%2Ftemplate%2Fhero-form-template.component.html)

## 模板驱动验证

为了往模板驱动表单中添加验证机制，你要添加一些验证属性，就像[原生的HTML表单验证器](https://developer.mozilla.org/en-US/docs/Web/Guide/HTML/HTML5/Constraint_validation)。`Angular`会用指令来匹配这些具有验证功能的指令。  

```html
<input id="name" name="name" class="form-control" 
      required minlength="4" appForbiddenName="bob"
      [(ngModel)]="hero.name" #name="ngModel" >

<div *ngIf="name.invalid && (name.dirty || name.touched)"
    class="alert alert-danger">

  <div *ngIf="name.errors.required">
    Name is required.
  </div>
  <div *ngIf="name.errors.minlength">
    Name must be at least 4 characters long.
  </div>
  <div *ngIf="name.errors.forbiddenName">
    Name cannot be Bob.
  </div>

</div>
```

注意以下几点:  

- `<input>`元素带有一些HTML验证属性: `required`和`minlength`。它还带有一个自定义的验证器指令`forbiddenName`。
- `#name="ngModel"`把`NgModel`导出成了一个名叫`name`的局部变量。`NgModel`把自己控制的`FormControl`实例的属性映射出去,让你能在模板中检查控件的状态，比如`valid`和`dirty`。
- `<div>`元素的`*ngIf`揭露了一套消息`divs`，但是只有在"name"错误和控制器为`dirty`或者`touched`.
- 每个嵌套的`<div>`为其中一个可能出现的验证错误显示一条自定义消息。比如`required`、`minlength`和`forbiddenName`

## 响应式表单的验证

在响应式表单中，真正的源码都在组件类中。不应该通过模板上的属性来添加验证器，而应该在组件类中直接把验证器函数添加到表单控件模型(FormControl)上。然后，一旦控件发生了变化，Angular就会调用这些函数。



**验证器函数**  

有两种验证器函数：`同步验证`和`异步验证器`。

- **同步验证器**函数接受一个控件实例，然后返回一组验证错误或 `null`。你可以在实例化一个 `FormControl` 时把它作为构造函数的第二个参数传进去。
- **异步验证器**函数接受一个控件实例，并返回一个承诺（Promise）或可观察对象（Observable），它们稍后会发出一组验证错误或者 `null`。你可以在实例化一个 `FormControl` 时把它作为构造函数的第三个参数传进去。



### 内置验证器

```js
ngOnInit(): void {
  this.heroForm = new FormGroup({
    'name': new FormControl(this.hero.name, [
      Validators.required,
      Validators.minLength(4),
      forbiddenNameValidator(/bob/i) // <-- Here's how you pass in the custom validator.
    ]),
    'alterEgo': new FormControl(this.hero.alterEgo),
    'power': new FormControl(this.hero.power, Validators.required)
  });

}

get name() { return this.heroForm.get('name'); }

get power() { return this.heroForm.get('power'); }
```

- `name`控件设置了两个内置验证器: `Validators.required`和`Validators.minLength(4)`。
- 由于这些验证器都是同步验证器,因此你要把它们作为第二个参数传递进去。
- 可以通过把这些函数放进一个数组后传递进去，可以支持多重验证器。
- 这个例子添加了一些getter方法。在响应式表单中，你通常会通过它们所属的控件组(FormGroup)的`get`方法来访问表单控件，但有时候作为模板定义一些`getter`作为简短形式。

```html
<input id="name" class="form-control"
      formControlName="name" required >

<div *ngIf="name.invalid && (name.dirty || name.touched)"
    class="alert alert-danger">

  <div *ngIf="name.errors.required">
    Name is required.
  </div>
  <div *ngIf="name.errors.minlength">
    Name must be at least 4 characters long.
  </div>
  <div *ngIf="name.errors.forbiddenName">
    Name cannot be Bob.
  </div>
</div>
```

关键改动是：  

- 该表单不再导出任何指令，而是使用组件类中定义的`name`读取器
- `required`属性仍然存在，虽然验证器不再需要它，但你仍然要在模板中保留它，以支持css样式可访问性。



## 自定义验证器

由于内置验证器无法适用于所有应用场景，有时候你还是得创建自定义验证器。

考虑前面的例子中的`forbiddenNameValidator`函数。该函数的定义看起来是这样的：

Forbidden-name.directive.ts:

```js
export function forbiddenNameValidator(nameRe: RegExp): ValidatorFn {
    return (control: AbstractControl): { [key: string]:any }| null => {
     	const forbidden = nameRe.test(control.value);
        return forbidden ? {'forbiddenName' : {value: control.value}} : null;
    }
}
```

### 在响应式表单中添加自定义验证器

在响应式表单组件中，添加自定义验证器相当简单。你所要做的一切就是直接把这个函数传给`FormControl`。

```js
this.heroForm = new FormGroup({
    'name': new FormControl(this.hero.name,[
        Validators.required,
        Validators.minLength(4),
        forbiddenNameValidator(/bob/i)		// 自定义验证器
    ]),
    'alterEgo': new FormControl(this.hero.alterEgo),
    'power': new FormControl(this.hero.power,Validators.required)
});
```

### 添加到模板驱动表单

在模板驱动表单中，你不用直接访问`FormControl`实例。所以不能像响应式表单中那样把验证器传进去，而应该在模板中添加一个指令。

`ForbiddenValidatorDirective`指令相当于`forbiddenNameValidator`的包装器。

Angular在验证流程中的识别出指令的作用,是因为指令把自己注册到了`NG_VALIDATORS`提供商中,该提供商有一组可扩展的验证器。

```js
providers: [{provide: NG_VALIDATORS, useExisting: ForbinddenValidatorDirective, multi: true}];
```

然后该指令实现了`Validator`接口，以便它能简单的与Angular表单集成在一起。这个指令的其余部分有助于你理解他们是如何协作的:  

```js
import { Directive, Input, OnChanges, SimpleChanges } from '@angular/core';
import { AbstractControl, NG_VALIDATORS, Validator, ValidatorFn, Validators } from '@angular/forms';

/** A hero's name can't match the given regular expression */
export function forbiddenNameValidator(nameRe: RegExp): ValidatorFn {
  return (control: AbstractControl): {[key: string]: any} | null => {
    const forbidden = nameRe.test(control.value);
    return forbidden ? {'forbiddenName': {value: control.value}} : null;
  };
}

@Directive({
  selector: '[appForbiddenName]',
  providers: [{provide: NG_VALIDATORS, useExisting: ForbiddenValidatorDirective, multi: true}]
})
export class ForbiddenValidatorDirective implements Validator {
  @Input('appForbiddenName') forbiddenName: string;

  validate(control: AbstractControl): {[key: string]: any} | null {
    return this.forbiddenName ? forbiddenNameValidator(new RegExp(this.forbiddenName, 'i'))(control)
                              : null;
  }
}
```

一旦`ForbiddenValidatorDirecitve`写好了，你只要把`forbiddenName`选择器添加到输入框上就可以激活这个验证器了。

```js
<input id="name" name="name" class="form-control" 
      required minlength="4" appForbiddenName="bob"
      [(ngModel)]="hero.name" #name="ngModel" >
```

### 表示控件状态的CSS类

像Angularjs中一样，Anuglar会自动把很多控件属性作为CSS类映射到控件所在的元素上。你可以使用这些类来根据表单状态给表单控件元素添加样式。目前支持下列类:  

- `.ng-valid`  - 控件的值**有效的**
- `.ng-invalid` - 控件的值**无效的**
- `.ng-pending` - 控件的值**等待中**
- `.ng-pristine` - 控件的值**未变化**
- `.ng-dirty` - 控件的值**变化了**
- `.ng-untouched`  - 控件**未被访问过**
- `.ng-touched` - 控件**被访问过**

一个例子：

```css
.ng-valid[required], .ng-valid.required  {
  border-left: 5px solid #42A948; /* green */
}

.ng-invalid:not(form)  {
  border-left: 5px solid #a94442; /* red */
}
```



## 交叉字段验证

表单具有如下结构:

```js
const heroForm = new FormGroup({
    'name': new FromControl(),
    'alterEgo': new FormControl(),
    'power': new FormControl()
})
```

注意,name和alterEgo是兄弟控件。要想在单个的自定义验证器中计算这两个控件，我们就得在他们共同的祖先控件(FormGroup)中进行验证。这样,我们就可以查询`FormGroup`的子控件，从而我们能够比较它们的值。

要想给`FormGroup`添加验证器，就要在创建时把一个新的验证器传给它们的第二个参数。

```js
const heroForm = new FormGroup({
    'name': new FromControl(),
    'alterEgo': new FormControl(),
    'power': new FormControl()
},{ validators: identityRevealedValidator });
```

验证器的代码如下：

identity-revealed.directive.ts:

```js
/** A hero's name can't match the hero's alter ego */
export const identityRevealedValidator: ValidatorFn = (control: FormGroup): ValidationErrors | null => {
  const name = control.get('name');
  const alterEgo = control.get('alterEgo');

  return name && alterEgo && name.value === alterEgo.value ? { 'identityRevealed': true } : null;
};
```

身份验证器实现`ValidatorFn`接口。它将Angular控件对象作为参数，如果表单有效则返回null，`ValidationErrors`否则返回null 。

我们先通过调用 `FormGroup` 的 [get](https://angular.cn/api/forms/AbstractControl#get) 方法来获取子控件。然后，简单地比较一下 `name` 和 `alterEgo` 控件的值。

如果这两个值不一样，那么英雄的身份就应该继续保密，我们可以安全的返回 null。否则就说明英雄的身份已经暴露了，我们必须通过返回一个错误对象来把这个表单标记为无效的。

接下来，为了提供更好的用户体验，当表单无效时，我们还要显示一个恰当的错误信息。



```html
<div *ngIf="heroForm.errors?.identityRevealed && (heroForm.touched || heroForm.dirty)" class="cross-validation-error-message alert alert-danger">
    Name cannot mathch alter ego.
</div>
```



### 添加到模板驱动表单中

首先，我们必须创建一个指令，它会包装这个验证器函数。我们使用 `NG_VALIDATORS` 令牌来把它作为验证器提供出来。如果你还不清楚为什么要这么做或者不能完全理解这种语法，请重新访问前面的[小节](https://angular.cn/guide/form-validation#adding-to-template-driven-forms)。



Identity-revealed.directive.ts:

```js
@Directive({
  selector: '[appIdentityRevealed]',
  providers: [{ provide: NG_VALIDATORS, useExisting: IdentityRevealedValidatorDirective, multi: true }]
})
export class IdentityRevealedValidatorDirective implements Validator {
  validate(control: AbstractControl): ValidationErrors {
    return identityRevealedValidator(control)
  }
}
```

接下来，我们要把该指令添加到 HTML 模板中。由于验证器必须注册在表单的最高层，所以我们要把该指令放在 `form`标签上。

```html	
<form #heroForm="ngForm" appIdentityRevealed>
```



`identity-revealed.directive.ts` 最终效果: 

```js
import { Directive } from '@angular/core';
import { AbstractControl, FormGroup, NG_VALIDATORS, ValidationErrors, Validator, ValidatorFn } from '@angular/forms';

/** A hero's name can't match the hero's alter ego */
export const identityRevealedValidator: ValidatorFn = (control: FormGroup): ValidationErrors | null => {
  const name = control.get('name');
  const alterEgo = control.get('alterEgo');

  return name && alterEgo && name.value === alterEgo.value ? { 'identityRevealed': true } : null;
};

@Directive({
  selector: '[appIdentityRevealed]',
  providers: [{ provide: NG_VALIDATORS, useExisting: IdentityRevealedValidatorDirective, multi: true }]
})
export class IdentityRevealedValidatorDirective implements Validator {
  validate(control: AbstractControl): ValidationErrors {
    return identityRevealedValidator(control)
  }
}
```



