<h1>pc开发问题记录</h1>

## 问题目录
<p>
  1、<a href="#t_1">pc端‘单页面应用’，两个页面切换时（若一个页面有滚动条，一个没有滚动条），出现屏幕闪动</a>
</p>
<p>
  2、<a href="#t_2">pc端 ajax请求文件下载</a>
</p>
<p>
  3、<a href="#t_3">纯前端将数据导出成Excel文件</a>
</p>


<hr>


### 问题描述
<p> 
  <a name="t_1">
    pc端‘单页面应用’，两个页面切换时（若一个页面有滚动条，一个没有滚动条），出现屏幕闪动
  </a>
</p>

### 解决：
```css
html {
    overflow-y: auto;
    overflow-x: hidden;
}
body {
    width: 100vw;
}
```

<hr>

### 问题描述
<p>
  <a name="t_2">
    pc端 ajax请求文件下载
  </a>
</p>

### 解决
#### --客户端
```javascript
function ajax(obj) {
    let xhr = new XMLHttpRequest();
    // 设置响应对象
    // 此处同样可以设置成arraybuffer也可以实现
    xhr.responseType = "blob";
    xhr.onreadystatechange = function () {
      if (xhr.readyState == 4) {
        if (xhr.status == 200) {
          obj.success(xhr.response);
        } else {
          obj.error(xhr.response);
        }
      }
    };
    xhr.open("post", "getFile/getImage.do");
    xhr.send(null);
  }

  function downlowFileHandler() {
    ajax({
      success(blob) {
      /* responseType设置成arraybuffer的时候
      success(data) {
      let blob = new Blob([data]);
      */
        let href = window.URL.createObjectURL(blob); //创建下载的链接
        let downloadElement = document.createElement('a');
        downloadElement.target = "blank";
        downloadElement.href = href;
        downloadElement.download = "test.png"; //下载后文件名
        document.body.appendChild(downloadElement);
        downloadElement.click(); //点击下载
        setTimeout(() => {
          document.body.removeChild(downloadElement); //下载完成移除元素
          window.URL.revokeObjectURL(href); //释放掉blob对象
        }, 300);
      },
      error(err) {
        console.log(err);
      }
    });
  }
```
#### --服务器端
```java
    @RequestMapping("/getImage")
      public void getImage(HttpServletResponse response) throws Exception {
        File file = new File("C:\\Users\\tanxin\\Desktop\\test.png");
        FileInputStream is = new FileInputStream(file);
        byte[] arr = new byte[(int) file.length()];
        is.read(arr, 0, (int) file.length());
        is.close();
        response.setContentType("image/png");
        response.getOutputStream().write(arr);
      }
```


<hr/>

### 问题描述
<p>
  <a name="t_3">
    纯前端将数据导出成Excel文件
  </a>
</p>

### 解决
```javascript
      /*
      * 生成数据
      * @param str:字符串
      * */
      function loadFile(str) {
        let blob = new Blob([str], {type: "application/vnd.ms-excel"});
        let fileName = "供应商数据.xls";
        // for IE;
        if (window.navigator && window.navigator.msSaveOrOpenBlob) {
          window.navigator.msSaveOrOpenBlob(blob, fileName);
        } else {
          let href = window.URL.createObjectURL(blob);
          let a = document.createElement("a");
          a.download = fileName;
          a.href = href;
          a.target = "blank";
          a.style = "display:none;";
          document.body.appendChild(a);
          a.click();
          document.body.removeChild(a);
          try {
            window.URL.revokeObjectURL(href);
          } catch (e) {
            console.log(e);
          }
        }
      }

      /*
      * 生成数据
      * @param data:数组数据
      * */
      function productLoadData(data) {
        let str = "<html><head><meta charset='utf-8'/><style>td{text-align: center;thead:{font-weight: bold;}}</style></head><body>" +
          "<table border='1px solid #333;'><head>" +
          "<tr>" +
          "<td>公司名称</td>" +
          "<td>统一社会信用代码</td>" +
          "<td>法人代表</td>" +
          "<td>注册资本(万元)</td>" +
          "<td>企业性质</td>" +
          "<td>经营范围</td>" +
          "<td>企业资源</td>" +
          "<td>联系人</td>" +
          "<td>联系电话</td>" +
          "<td>入库时间</td>" +
          "<td>供应商等级</td>" +
          "</tr></head><tbody>";
        data.forEach((item, i, arr) => {
          str += "<tr>" +
            "<td>" + item.customName + "</td>" +
            "<td>" + item.socialCreditCode + "</td>" +
            "<td>" + item.legelPeopleName + "</td>" +
            "<td>" + item.registeredCapital + "</td>" +
            "<td>" + item.companyNatureName + "</td>" +
            "<td>" + item.businessScope + "</td>" +
            "<td>" + item.resourceName + "</td>" +
            "<td>" + item.bcName + "</td>" +
            "<td>" + item.bcMob + "</td>" +
            "<td>" + item.createdDate + "</td>" +
            "<td>" + (item.suppLevel || "-") + "</td>" +
            "</tr>";
        });
        str += "</tbody></table></body></html>";
        return str;
      }
```
