## 自定义水印添加
***
一款声明版权立场的组件
该组件参考了 [315500033@qq.com](https://ext.dcloud.net.cn/plugin?id=362) 大神的组件 修改后符合自己的编码习惯 让水印有更多的展示方式

### 使用方式
#### 在 ``script`` 中引入组件：
```javascript
	import wmWatermark from '../../components/wm-watermark.vue';
	export default {
        components: {wmWatermark}
    }
```

#### 在 ``template`` 中使用组件:

```html
	<wm-watermark text="这是水印信息"></wm-watermark>
```

### **属性说明**

| 属性 | 类型 | 默认值 | 说明 |
| :---: | :---: | :---: | :--------: |
| text | String | 无 | 设置水印文字 注：(文字优先图片) |
| imgUrl | String | 无 | 设置水印图片链接 注：(文字优先图片) |
| opacity | Number,String | 0.6 | 设置显示透明度 |
| num | Number | 20 | 设置水印数量 |

