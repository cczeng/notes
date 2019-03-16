# Angular service ngOnDestroy

Angular service 也是有 ngOnDestroy 钩子的.

可以在 service 销毁后清除一些动作，例如 `subscribe`  

```javascript
import { OnDestroy } from '@angular/core';

export class appService implements OnDestroy {
    ngOnDestroy() {
        // 取消订阅
        this.ds.dsInstance.presence.unsubscribe();
    }
}
```