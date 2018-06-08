# keydown shift and space - 监听键盘组合键(shift + space)

监听键盘按下的事件:   

<p style="color:red">注意: keydown 事件会多次触发,所以需要判断是否为重复触发</p>   

`e.repeat` 可以判断是否为重复触发.

```javascript
@HostListener('window:keydown', ['$event'])
  keydownPTT(event: KeyboardEvent) {
    if (event.shiftKey && event.keyCode == 32 && !event.repeat) {
      console.log(`按下了shift和和空格键`);
    }
  }
  @HostListener('window:keyup', ['$event'])
  keyupClosePTT(event: KeyboardEvent) {
    if (event.shiftKey && event.keyCode == 32) {
      console.log(`松开按键,停止语音上传`);
    }
  }
```