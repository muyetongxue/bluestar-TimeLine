# bluestar-TimeLine

## 时间轴

## 引入

在script标签中引入组件

```typescript
//使用HBuilderX导入插件
import TimeLine from "@/uni_modules/bluestar-TimeLine/components/bluestar-TimeLine/bluestar-TimeLine.vue"

//使用npm install
import TimeLine from "bluestar-timeline/components/bluestar-TimeLine/bluestar-TimeLine.vue"
```

## 代码演示

```vue
<template>
	<TimeLine beginDate="2022-03-22" endDate="2022-03-28" @callback="dateEvent"></TimeLine>
</template>

<script setup lang="ts">
const dateEvent = (obj : object) => {
	console.log(obj)
}
</script>
```

## API

#### Props

| 参数        | 说明     | 类型        | 默认值   |
|-----------|:-------|:----------|:------|
| beginDate | 开始日期   | `string`  | -     |
| endDate   | 截止日期   | `string`  | -     |
| reverse   | 是否倒序排列 | `boolean` | false |
| color     | 主题色    | `string`  | #0E75FC |

#### Events

| 事件名称 | 说明      | 回调参数 |
| :----- |:--------|------|
| bing:callback | 点击选项时触发 | 选中值  |

## 作者

![](https://img.shields.io/static/v1?label=蓝星软件&message=@caisheng&labelColor=0E75FC)
