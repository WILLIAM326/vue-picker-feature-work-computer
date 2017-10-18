# picker

> A Vue.js project

## Overview
vue-picker 是在mint picker组件基础上 添加部分功能，使其完美的，更加符合自己的业务需要

## 安装
不需要安装什么，把picker文件下载下来就可以了
样式 js 动画js都在里面了

```
git clone https://github.com/Jaction/vue-picker.git
```

```
import mtPicker from 'src/components/picker/index';
```

Register component:

```
Vue.component('picker', Picker);
```

## 使用

#### 普通下拉框
```
<mPicker :slots='slots' :isPicker='isPicker' :indexText='indexText' 
        :datakey='datakey' :valueKey='valueKey' 
        @confirm='pickerConfirm' @cancel='pickerCancel'>
</mPicker>
```

```
import mPicker from 'components/picker/index'
export default {
    data() {
        return {
            slot: []
            isPicker: false // 显示隐藏
            indexText: '选择器' // 名称
            datakey: 'time' // 确定后返回v值，多个选择器是，数据是对象 v是key，方便赋值
            valueKey：'v', //作为文本显示在 Picker 中的对应字段的字段名
            info: {
                time
            } 
        }
    },
    components:{
        mtPicker
    },
    created() {
        // 页面多个普通下拉框(非联动,)，需要改变数组，必须在created赋值, 然后就可以在每个方法赋值
        this.slot = [{values: [{k:01, v: '好'}, {k: 02, v: '不好'}]}]
    },
    methods: {
        pickerCancel() {
            this.isPicker = false
        },
        pickerConfirm(value, key) {
            console.log(value, key); // {k: 01, v: '"好"'} dateTime
            // key就是time
            this.info[key] = value[valueKey];
        }
    }
}
```

#### 日期选择
```
<m-date-picker :isPicker='isDatePicker' :datakey='dateDatakey' 
        @confirm='datePickerConfirm' @cancel="datePickerCancel">
</m-date-picker>
```

```
import mDatePicker from 'src/components/picker/date-picker.vue';
export default {
    data() {
        return {
            isDatePicker: false,
            dateDatakey: 'dataTime', // 非必填
            info: {
                dataTime: ''
            }
        }
    },
    components:{
        mDatePicker
    },
    datePickerCancel() {
        this.isDatePicker = false;
    },
    datePickerConfirm(value, key) {
        console.log(value, key) // [2017, "01", "01"] "dateTime"
        // key === this.dateDatakey 'dateTime'
        this.isDatePicker = false;
        this.info[key] = value.join('-'); // 2017-01-01
    },
}
```

#### 地区选择
```
<m-area-picker :isPicker='isAreaPicker' :datakey='areaDataKey' 
        @confirm='areaPickerConfirm' @cancel="areaPickerCancel">
</m-area-picker>
```

```
import mAreaPicker from 'src/components/picker/area-picker.vue';
export default {
    data() {
        return {
            isAreaPicker: false,
            datakey: 'arae', // 非必填
            info: {
                area: ''
            }
        }
    },
    areaPickerCancel() {
        this.isAreaPicker = false;
    },
    areaPickerConfirm(value, key) {
        console.log(value); // {k: "110000-110100-000046", v: "北京市-北京市-北京技术开发区"}
        // key === this.datakey 'area'
        this.isAreaPicker = false;
        this.info[key] = value.v;
    }
}
```

2017-09-21补充


## 补充 picker选择器

#### 非联动picker

```
import mPicker from 'src/components/picker/index';
```

```
<m-picker :slots='slots' :isPicker='isPicker' 
    :indexText='indexText'
    :datakey='datakey'
    @confirm='pickerConfirm' @cancel='pickerCancel'>
    </m-picker>
```



###### API

| 参数 | 说明 | 类型 | 默认值 |
| --- | --- | --- | --- |
| slots | slot 对象数组 | Array | [] |
| valueKey | 当 values 为对象数组时，作为文本显示在 Picker 中的对应字段的字段名,业务需求默认’v’ | String | ‘v' |
| indexText | 选择器名称 | String | ‘请选择' |
| datakey  | 数据对象的key，选择器确定后confrim第二个参数，原本带出 | String | ‘’ |
| @confirm | 确定事件，默认两个参数，第一个是选择的对象数组，第二个是datakey | event |  |
| @cancel | 取消选择器，自行隐藏组件 | event  |  |


<br>

#### 日期选择器(年月日) 

```
import mAreaPicker from 'src/components/picker/area-picker.vue';
```
```
<m-date-picker :isPicker='isDatePicker' :datakey='dateDatakey'
    @confirm='datePickerConfirm' @cancel="datePickerCancel">
    
</m-date-picker>
```
###### API

| 参数 | 说明 | 类型 | 默认值 |
| --- | --- | --- | --- |
| indexText | 选择器名称 | String | ‘请选择日期' |
| datakey  | 数据对象的key，选择器确定后confrim第二个参数，原本带出 | String | '' |
| @confirm | 确定事件，默认两个参数，第一个是选择的数组[2017,01,01]，第二个是datakey | event |  |
| @cancel | 取消选择器，自行隐藏组件 | event  |  |

<br>
### 城市选择器

```
import mAreaPicker from 'src/components/picker/area-picker.vue';
```

```
<m-area-picker :isPicker='isAreaPicker' isZone :datakey='areaDataKey'
    @confirm='areaPickerConfirm' @cancel="areaPickerCancel">
</m-area-picker>
```

###### API

| 参数 | 说明 | 类型 | 默认值 |
| --- | --- | --- | --- |
| indexText | 选择器名称 | String | ‘请选择地区' |
| datakey  | 数据对象的key，选择器确定后confrim第二个参数，原本带出 | String | '' |
| isZone  | 只需省市，不需要区 | Boolean | false |
| @confirm | 1、确定事件，默认两个参数，第一个是选择的数组[2017,01,01]，第二个是datakey;<br> 2、当`isZone: true`,不需要区，但第三个参数会把区带出，自行操作 | event |  |
| @cancel | 取消选择器，自行隐藏组件 | event  |  |


``` 

## Build Setup

``` bash
# install dependencies
npm install

# serve with hot reload at localhost:8080
npm run dev

# build for production with minification
npm run build

# build for production and view the bundle analyzer report
npm run build --report
```

For detailed explanation on how things work, checkout the [guide](http://vuejs-templates.github.io/webpack/) and [docs for vue-loader](http://vuejs.github.io/vue-loader).
