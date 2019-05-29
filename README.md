# MpVue 时间日期选择器
因为业务需要一个可以同时选择日期和时间的组件,利用小程序自带的[picker](https://developers.weixin.qq.com/miniprogram/dev/component/picker.html)组件封装

# 使用说明

| 名称|描述
| -------------- | -------------------------------------------------------------------------------------------- |
| `onChange` | [说明]：滑动选择时间后会触发此事件
| `onConfrim` | [说明]：点击确定后会触发此事件|

```javascript
// 引入组件
<picker-date ref="picker"  @onChange="onChange"></picker-date>

// 调用
this.$refs.picker.show()
```
# 截图
![截图](https://s2.ax1x.com/2019/05/29/VnB6BD.gif)