# 生命周期钩子  

![ionic 生命周期钩子](http://blog.ionic.io/blog/wp-content/uploads/2016/11/ionicViewLifecycleEvents.png)

  

## 加载  



- `ionViewDidLoad` ： **仅在视图存储在内存中时触发**。输入已缓存的视图时不会触发此事件。这是init相关任务的好地方。  
- `ionViewWillEnter`：在进入页面**之前，它在成为活动**页面**之前被**触发。每次进入视图（设置事件监听器，更新表等）时，都可以将它用于您想要执行的任务。  
- `ionViewDidEnter`：进入页面**后**，**在成为活动页面后**触发。与前一个非常相似。   
- `ionViewWillLeave`：当你离开页面时，**在它停止活动之前被**触发。将它用于每次离开页面时需要运行的内容（停用事件监听器等）。   
- `ionViewDidLeave`：当你离开页面时，**在它停止活动之后被**触发。与前一个类似。   
- `ionViewWillUnLoad`：**在完全删除视图时**（在离开非缓存视图后）触发。   



## 导航    

- `ionViewCanEnter` :在进入视图之前触发，允许您控制是否可以访问视图（返回*true*或*false*）。  
- `ionViewCanLeave`：在离开视图之前触发，允许您控制是否可以保留视图。   



