# work踩坑

## 2019-07-17

* 缩略图展示，等比压缩图片会变形，PM要求采用裁剪模式
* 即image设置mode='aspectFill'，缩放模式，长边截取

* token存储在globalData里面，小程序关闭，token丢失
* 解决方法：token存储在storage里面
* 小程序中：globalData为内存，storage为缓存
