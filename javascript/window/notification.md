# [Notification 系统弹窗](https://developer.mozilla.org/zh-CN/docs/Web/API/notification)

在有些时候我们想要调用系统的通知来提醒用户，就需要使用到 `Notification` 方法   

构造方法:   
```javascripe
const notification = new Notification(title, { dir?, lang?, body?, tag?, icon?});
```
> 参数说明  

`title`:  显示通知的标题，必须.   
`options`: 可选参数,包含如下信息   
- `dir`: 文字的方向; `auto(自动)`,`ltr(从左到右)`,`rtl(从右到左)`
- `lang`: 通知中所使用的语言.
- `body`: 通知额外显示的字符串,其实就是body内容
- `tag`: 赋予通知一个ID，以便在必要的时候对通知进行刷新、替换或移除
- `icon`: 一个图片的URL,将被用于显示通知的图标

以 `Angular5` 为实例:    
```javascript
import { Injectable } from '@angular/core';

import * as _ from 'lodash';
declare var Notification: any;

@Injectable()
export class SystemNotificationService {

  constructor() {
    // 检查是否已经请求过权限
    Notification.requestPermission().then(result => {
      if (result === 'denied') {
        console.log(`用户拒绝发送通知`);
        return;
      }
      if (result === 'default') {
        console.log(`还未被询问通知.尝试请求`);
        return;
      }
    });
  }

  sendSystemNotification(title: string, body: string, time?: number, icon?: string, dir?: string, tag?: string) {
    if (!('Notification' in window) && Notification.permission != 'granted') return;
    const options = { title, body, time, icon, dir, tag };
    let notification = new Notification(title, _.pickBy(options, _.identity));
    time && this.close(time, notification);
  }

  // 关闭
  close(time: number, notification?) {
    if (notification) {
      notification.onshow = () => { setTimeout(() => { notification.close().bind(notification) }, time) };
    } else {
      Notification.close();
    }
  }
}
```

这里使用到 `lodash` 这个库.