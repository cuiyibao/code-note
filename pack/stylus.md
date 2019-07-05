## 关闭stylus编译自动添加前缀的功能

For example, consider the following:
```
@keyframes foo {
  from {
    color: black
  }
  to {
    color: white
  }
}
```
This expands to our three default vendors, and the official syntax:
```
@-moz-keyframes foo {
  from {
    color: #000;
  }
  to {
    color: #fff;
  }
}
@-webkit-keyframes foo {
  from {
    color: #000;
  }
  to {
    color: #fff;
  }
}
@-o-keyframes foo {
  from {
    color: #000;
  }
  to {
    color: #fff;
  }
}
@keyframes foo {
  from {
    color: #000;
  }
  to {
    color: #fff;
  }
}
```
If we wanted to limit to the official syntax only, simply alter vendors:
```
vendors = official

@keyframes foo {
  from {
    color: black
  }
  to {
    color: white
  }
}
```
Yielding:
```
@keyframes foo {
  from {
    color: #000;
  }
  to {
    color: #fff;
  }
}
```