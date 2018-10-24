# @puge/clock


> 时钟组件
![Alt text](http://github-puge.oss-cn-beijing.aliyuncs.com/time/time.png)

## 安装 及 使用

```bash
$ npm install --save @puge/time
```

使用组件

```vue
<template>
  <Clock :fontSize="20" color="#FFFFFF" :allDate="true">
</template>

<script>
  import Clock from '@puge/time'
  export default {
    components: {
      Clock
    }
  }
</script>
```

### 参数

#### fontSize
类型: `Number`<br>
必要性: `false`<br>
默认值: 20

字体大小

#### color
类型: `String`<br>
必要性: `false`<br>
默认值: `#FFFFFF`

字体颜色

#### allDate
类型: `Boolean`<br>
必要性: `false`<br>
默认值: false

是否显示完整时间

### 事件

input 编辑结束事件
