# 微信支付6位密码框

解决方案:    
   

```html
<div class="input-key">
    <div class="input-key__view">
        <span>
        <input type="{{defaultWechatConfig.inputType}}" maxlength="1" value="{{_inputValue[0]}}" disabled>
        </span>
        <span>
        <input type="{{defaultWechatConfig.inputType}}" maxlength="1" value="{{_inputValue[1]}}" disabled>
        </span>
        <span>
        <input type="{{defaultWechatConfig.inputType}}" maxlength="1" value="{{_inputValue[2]}}" disabled>
        </span>
        <span>
        <input type="{{defaultWechatConfig.inputType}}" maxlength="1" value="{{_inputValue[3]}}" disabled>
        </span>
        <span>
        <input type="{{defaultWechatConfig.inputType}}" maxlength="1" value="{{_inputValue[4]}}" disabled>
        </span>
        <span>
        <input type="{{defaultWechatConfig.inputType}}" maxlength="1" value="{{_inputValue[5]}}" disabled>
        </span>
    </div>
    <input type="{{defaultWechatConfig.inputType}}" #inputPwd class="input-key__input" (focus)="_inputValue='';_isError=false;"
        [(ngModel)]="_inputValue" (ngModelChange)="checkValue($event)" maxlength="6" autofocus>
    </div>
```

> 说明： `用一个最大的输入框设置为隐藏`,用户的输入其实是在这个输入框中,然后用6个小的输入框,分别的值为大输入框的不同位数的值.  