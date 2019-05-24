## canvas生成海报
***
canvas生成海报,适用于商城海报分享功能  
一款基于canvas的商品海报生成组件，可以根据图片比例生成商品海报图
    
### 使用方式：
#### 在 ``script`` 中引用组件:
```javascript
    import wmPoster from "@/components/wm-poster/wm-poster.vue"
    export default {
        components: {wmPoster}
    }
```

#### 在 ``template`` 中适用组件:

```html
    <wm-poster imgSrc="图片链接地址" QrSrc="二维码链接地址" Title="标题文本" PriceTxt="价格显示" OriginalTxt="划线价显示"></wm-poster>
```

### **属性说明:**

| 属性 | 类型 | 默认值 | 说明 |
| :---: | :----: | :----: | :----: |
| CanvasID | String | PosterCanvas | 等同于canvas-id |
| imgSrc | String | 无 | 商品展示图链接 |
| QrSrc | String | 无 | 二维码或太阳码链接 |
| Title | String | 无 | 商品展示标题文本 |
| TitleColor | String | #333333 | 商品展示标题颜色 |
| LineType | [String,Boolean] | true | 标题显示行数控制,  true(最多显示两行多余用...),  false(不做限制) |
| PriceTxt | String | 99.99 | 商品销售价 |
| PriceColor | String | #e31d1a | 商品销售价颜色 |
| OriginalTxt | String | 199.99 | 商品划线价 |
| OriginalColor | String | #b8b8b8 | 默认颜色（如划线价与扫描二维码提示颜色） |
| Width | String | 750 | canvas画布宽度  (高度根据图片比例计算 单位upx) |
| CanvasBg | String | #ffffff | canvas画布背景色 |
| Referrer | String | quitter推荐给你 | 推荐人信息 |
| ViewDetails | String | 长按或扫描识别二维码 | 描述提示文字 |

### **事件说明:**

| 事件名称 | 说明 |
| :---: | :---: |
| success | 海报生成成功返回状态（包含生成的图片地址） |