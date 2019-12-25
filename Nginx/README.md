<h1>nginx配置问题记录</h1>

## 问题目录
<p>
  1、<a href="#t_1">nginx禁止缓存</a>
</p>
<p>
  2、<a href="#t_2">nginx配置代理</a>
</p>





### 1、问题描述
<p> 
  <a name="t_1">
    禁止缓存，在location下添加header可以禁止缓存
  </a>
</p>

### 解决：
```text
  location /dist/index.html {
	    add_header "Cache-Control" " no-cache, no-store, must-revalidate";
	 }
```

### 2、问题描述
<p> 
  <a name="t_2">
    配置反向代理：api:是匹配的标识，rewrite_api:是复写，proxy_pass 是代理的的地址
  </a>
</p>

### 解决：
```text
  location /api{
  	   rewrite      ^/api/(.*)$  /rewrite_api/$1 break;
  	   proxy_pass   http://10.19.0.42:8001;
  	}
```
