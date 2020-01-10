# ios问题记录

## 问题目录
<p>
  1、<a href="#t_1">ios中使用iframe时，子页面存在样式放大的情况</a>
</p>

<p>
  2、<a href="#t_2">ios中固定定位的层级问题</a>
</p>

<p>
  3、<a href="#t_3">ios中使用iframe时，子页面中输入框的问题</a>
</p>

<hr>

### 1.1问题描述
<p> 
  <a name="t_1">
    ios中使用iframe时，子页面存在样式放大的情况
  </a>
  <br>
  参考地址<br>
  1：https://www.cnblogs.com/sichaoyun/p/11535407.html<br>
  2：https://blog.csdn.net/weixin_44141265/article/details/88642018
</p>

### 1.2解决：
```javascript
  let system = window.userAgent;
  iframe_section_html = '';
  if (system == "iOS") {
      iframe_section_html = '<iframe class="iframe_style" ' +
        'height="100%" ' +
        'scrolling="no" ' +
        'style="width: 1px; min-width: 100%; *width: 100%;" ' +
        'src="' + url + '" ' +
        'frameborder="0"></iframe>';
  } else {
    iframe_section_html = '<iframe src="' + url + '" scrolling="auto" frameborder="0" width="100%" height="100%"></iframe>';
  }
```

### 2.1问题描述
<p>
  <a name="t_2">
  ios中 固定定位的父级元素存在posisiont:relative;
  或则-webkit-overflow-touch:touch;的情况会与【祖辈级元素的子级定位元素】存在的z-index深度的问题
  </a>
</p>

### 2.2解决：
```
  1、如果存在固定定位的情况，避免其父级使用position：relative;和-webkit-overflow-touch:touch;
  2、避免使用fixed定位
```


### 3.1问题描述
<p>
  <a name="t_3">
    苹果手机中运行的页面，在iframe 中 ，手指点击input框，自动弹出键盘后，输入几个文字，然后手指再点击一下input框之后或者拖动页面，再在键盘里敲字母，就无法在input框显示所敲入的内容了。
  </a>
  <br>
  参考地址：https://blog.csdn.net/baidu_29701003/article/details/80531836
</p>

### 3.2解决
```javascript
document.body.addEventListener('keydown', (e) => {
        if (e.target.nodeName === 'INPUTE' || e.target.nodeName === 'TEXTAREA') {
          window.focus();
          e.target.focus();
        }
      }, false);
```
