<h1>vue问题记录问题记录</h1>

## 问题目录
<p>
  1、<a href="#t_1">退出登录后删除keep-alive中保存的数据</a>
</p>
<p>
  2、<a href="#t_1">标题</a>
</p>





### 问题描述
<p> 
  <a name="t_1">
    退出登录后删除keep-alive中保存的数据
  </a>
</p>

### 解决：
```vue
<tempalte>
  <keep-alive :include="cacheList">
    <router-view/>
  </keep-alive>
</tempalte>

<script>
/**
* 说明：登录、退出等情形可以通过通知 cacheList 变量来控制 缓存
*/
  export default {
    data() {
      return {
        // 需要缓存的
        needCacheList: [
          'homeComp', // 主页
          'userCenter', // 用户中心
        ],
        // 缓存数据
        cacheList: [],
      }
    },
    methods:{
      /*
      * 添加缓存
      * */
      addRouterCache() {
        // 添加后，会缓存添加的页面
        this.cacheList = [...this.needCacheList];
      },

      /*
      * 删除缓存
      * */
      delRouterCache() {
        // 清空会自动清除之前已经存在的缓存
        this.cacheList = [];
      },
    }
  }
</script>
```

<hr/>

### 问题描述
<p> 
  <a name="t_1">
    标题
  </a>
</p>

### 解决：
```javascript

```
