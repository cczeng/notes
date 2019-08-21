# [Accessibility 无障碍](https://material.angular.io/cdk/a11y/overview)   

> Angular CDK Accessibility 主要是放置一些方便使用者互动的功能，例如键盘交互。



## 导入   



导入模块：  

```js
import { A11yModule } from '@angular/cdk/a11y';

@NgModule({
    exports: [
        A11yModule
    ]
})
export class SharedMOdule {}
```

 

## 理解  



### ListKeyManager  

**ListKeyManager**是用来管理一组元件，让这些元件可以跟键盘互动，例如： `Tab`  `Esc`  `Enter`和`方向键`这些操作，但不是每个元件都支持这样的键盘互动功能，而我们则可以透过使用**ListKeyManager**，来让我们的元件轻易地连到跟键盘互动的成果！    



`ListKeyManager` 是一个基础类别，目前有两种延伸：   

- `FocusKeyManager`: 用于切换不同元件间在浏览器里的`focus`状态。这些元件需要实现以下`interface`: 

  ```typescript
  interface FocusableOption extends ListKeyManagerOption {
      focus(): void;
  }
  ```

- `ActiveDescendantKeyManager`: 用来切换`active`的模式，主要是在使用屏幕阅读程序时使用。 

  ```typescript
  interface Highlightable extends ListKeyManagerOption {
      setActiveStyles(): void;
      setInactiveStyles(): void;
  }
  ```



而这两个  `interface`都继承自`ListKeyManagerOption`，它的内容如下：  

```typescript
interface ListKeyManagerOption{
    disabled?: boolean;
    getLabel?(): string;
}
```

也就是说，当我们希望自定义的组件可以被`ListKeyManager`使用时，至少需要保证实现对应`interface`的接口功能，不过`ListKeyManagerOption`的内容不是必须的。    

当我们要使用`FousKeyManager`时，所有包含在内的元件都必须要实现  `focus()` 方法，但`ListKeyManagerOption` 的内容则不是必要的。    



## 使用  

一般来说，要使用`ListKeyManager`功能有3个步骤：    

1. 使用`@ViewChildren`查出页面上需要包含在内的元件 
2. 建立一个新的`ListKeyManager`，并把上一步查出来的清单当参数传入
3. 使用相关的键盘事件以及设置状态的方法，来连到互动效果  



**使用FocusKeyManager**   

由于需要使用`@ViewChildren`来取得一系列包含`focus()`方法的元件，最简单的方式是建立一个`directive`，并实现`ListKeyManagerOption`的`focus()`方法：    

`Keyboard-interactive-input`:   

```typescript
@Directive({
    selector: '[appKeyboardInteractiveInput]'
})
export class KeyboardInteractiveInputDriective implements FocusableOtion {
    constructor(private element: ElementRef){}
    
    focus(){
        this.element.nativeElement.focus();
    }
}
```



将要加入`FocusKeyManager`的元素加上自定的指令：   

```html
<div>
    <input appKeyboardInteractiveInput name="name" formControlName="name" maxlength="5" required>
</div>
```

  

**使用`@ViewChildren`查找到元件并加入`FocusManager`**    

接着我们使用`@ViewChildren`找出前面加入的`KeyboardInteractiveInputDriective`，并取得一个`QueryList`，然后在`ngAfterViewInit`钩子中把这个`QueryList`加入`FocusManager`：    

```typescript
@Component({
    //...
})
export class SurveyComponent implements AfterViewInit {
    @ViewChilren(KeyboardInteractiveInputDriective) KeyboardInteractiveInput: QueryList<KeyboardInteractiveInputDriective>;
    keyManager: FocusKeyManager<KeyboardInteractiveInputDriective>;
    
    ngAfterViewInit(){
        this.keyManager = new FocusKeyManager(this.KeyboardInteractiveInput).withWrap();
        this.keyManager.setActiveItem(0);
    }
}
```



**与键盘互动**：   

```typescript
import { UP_ARROW, DOWN_ARROW } from '@angular/cdk/keycodes';

@Component({
    //...
})
export class Component implements OnInit, AfterViewInit {
    //...
    
    @HostListener('keydown', ['$event'])
    keydown(event: KeyboardEvent){
        if(event.keyCode === UP_ARROW){
            this.keyManager.setPreviousItemActive();
        }else if(event.keyCode === DOWN_ARROW){
            this.keyManager.setNextItemActive();
        }else{
            this.keyManager.onKeydown(event);
        }
    }
}
```

  

## CdkFocusTrap    

> `CdkFocusTrap` 在  `dialog` 里，使用 Tab/Shift+Tab 切换时，是不会跳到dialog之外的。这样的功能就是透过 FocusTrap 提供的一系列 `directives`，来连接到目标。    



- `cdkTrapFocus`: 用来形成一个FocusTrap区域，一般情况下，使用Tab将无法跳出 
- `cdkFocusRegionStart`: FocusTrap 的范围起点
- `cdkFocusRegionEnd`: FocusTrap 的范围终点 
- `cdkFocusInitial`: 期间出现时，一开始会 `focus`的来源   

 

这里需要注意几点：   

1. `CdkFocusTrap`主要是用在**非静态**的区域，也就是由应用程序判断产生的，例如`*ngIf`，当显示的时候，就会进入 FocusTrap 的范围内
2. 也可以使用在静态的地方，但是会产生一个问题，当Tab进入这个区域时，会从  `cdkFocusRegionEnd` 开始，而非  `ckdFocusRegionStart` 或`cdkFocusInitaial`   



```html
<div cdkTrapFocus cdkTrapFocusAutoCapture="true" *ngIf="displayFocusTrap">
    <input value="1" cdkFocusInitial>
    <input value="2" cdkFocusRegionStart>
    <input value="3">
    <input value="4" cdkFocusRegionEnd>
</div>
```



效果如下：   

![FocusTrap Directive](https://wellwind.idv.tw/blog/2018/01/13/angular-material-26-cdk-accessibility/05-focus-trap-good.gif)

   

### FocusTrapFactory    

`cdkFocusTrap`用于产生一个 focus 区域，而  `FocusTrapFactory`则是真正背后在做事的`service`。包括dialog，也是使用`FocusTrapFactory`来产生focus区域的。   



```typescript
export class MatDialogContainer {
    constructor(
    	private _elementRef: ElementRef,
        private _focusTrapFactory: FocusTrapFactory
    ){}
    
    private _trapFocus(){
        if(!this._focusTrap){
            this._focusTrap = this._focusTrapFactory.create(this._elementRef.navtiveEelement);
        }
    }
    
    _onAnimationDone(event: AnimationEvent){
        if(event.toState === 'enter'){
            this._trapFocus();
        }else if(event.toState === 'exit') {
            this._restoreFocus();
        }
    }
}
```

  



## FocusMonitor    

> FocusMonitor 是一个用来检查元件是否被 focus 的 service，可以通过这个来监听某个元件的 focus 状态，以及focus来源是什么类型: `touch` `mouse` `keyboard` `program`   

```javascript
startMonitor(){
    console.log('start monitoring element');
    this.focusMonitor.monitor(this.container.nativeElement,this.renderer2,false).subscribe(mode => {
        console.log('element focused by ${mode}');
    });
}

stopMonitor(){
    this.focusMonitor.stopMonitoring(this.container.nativeElement);
    console.log('stop monitoring element');
}
```



## cdkMonitorElementFocus/cdkMonitorSubtreeFocus    

这是`FocusMonitor`对应的指令。如果只需要针对不同的focus状态来改变样式的时候，可以使用`cdkMonitorElementFocus`或`cdkMonitorSubtreeFocus`这两种directive:    

- `cdkMonitorElementFocus`: 只有自己被focus时有用
- `cdkMonitorSubtreeFocus`: 里面元素被focus时也有用    

```html
<div class="demo-focusable" tabindex="0" cdkMonitorElementFocus>
    <p>
        div with element focus monitored
    </p>
    <button>
        1
    </button>
    <button>
        1
    </button>
</div>
<div class="demo-focusable" tabindex="0" cdkMonitorSubtreeFocus>
    <p>
        div with subtree focus monitored
    </p>
    <button>
        1
    </button>
    <button>
        1
    </button>
</div>
```

当加上directive的元素被focus时候，会自动加上一个css class: `cdk-focused`     

另外，针对不同的focus来源，也会加上以下CSS Class:   

- `ckd-mouse-focused`
- `cdk-keyboard-focused`
- `cdk-program-focused`
- `cdk-touch-focused`    



## InteractivityChecker   

`InteractivityChecker` 是一个用来检查元件互动的service,透过这个service，我们可以得知某个元件是否支持浏览器的某种互动，包含以下检查:    

- `isDisabled`: 是否可以加上`disabled`属性
- `isFocusable`: 是否可以focus
- `isTabbable`: 使用Tab是否可以触碰到目标
- `isVisible`: 是否在书面上可以被看到    

```typescript
export class AppComponent implements OnInit,AfterContentInit {
    @ViewChild('button') button: ElementRef;
    
    constructor(private interactivityChecker: InteractivityChecker){}
    
    ngOnInit(){
        console.log(this.interactivityChecker.isDisabled(this.button.nativeEleemnt));
    }
}
```

  



## LiveAnnouncer

用于给阅读器屏幕上发送消息，让屏幕阅读念出的文字:   

```typescript
@Component({})
export class Component {
    constructor(liveAnnouncer: LiveAnnouncer){
        liveAnnouncer.announce('Hey Google!');
    }
}
```

