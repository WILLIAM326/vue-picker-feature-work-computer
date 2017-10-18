<template>
  <section>
    <div class="cell-btn">
      <button @click='loanBreedEvent'>下拉框1</button>
    </div>
    <div class="cell-btn">
      <button @click='loanPlanEvent'>下拉框2</button>
    </div>
    <div class="cell-btn">
      <button @click='dateTimeSeleted'>日期选择器</button>
    </div>
    <div class="cell-btn">
      <button @click='areaSeleted'>地区选择器</button>
    </div>
    {{info.loanBreed}}<br>
    {{info.loanPlan}}<br>
    {{info.dateTime}}<br>
    {{info.areaData}}
    <m-picker :slots='slots' :isPicker='isPicker' :indexText='indexText'
    :datakey='datakey' :valueKey='valueKey'
    @confirm='pickerConfirm' @cancel='pickerCancel'>
    </m-picker>
    <m-date-picker :isPicker='isDatePicker' :datakey='dateDatakey'
    @confirm='datePickerConfirm' @cancel="datePickerCancel">
    </m-date-picker>
    <m-area-picker :isPicker='isAreaPicker' :datakey='areaDataKey'
    @confirm='areaPickerConfirm' @cancel="areaPickerCancel">
    </m-area-picker>
  </section>
</template>

<script>
import mPicker from './picker/index';
import mDatePicker from './picker/date-picker.vue';
import mAreaPicker from './picker/area-picker.vue';
export default {
  name: 'picker',
  data() {
    return {
      isPicker: false, // 普通选择器显示或隐藏
      isDatePicker: false, // 时间选择器显示或隐藏
      isAreaPicker: false, // 地区选择器显示或隐藏
      valueKey: 'v', // 下拉框设置 value-key 属性来指定显示的字段名
      indexText: '贷款品种', // 选择器名称
      isSell: true,
      datakey: '', // 选择器结果赋值到对象的key值
      dateDatakey: 'dateTime',
      areaDataKey: 'areaData',
      slots: [], // slot 对象数组
      msg: '',
      info: {
        loanBreed: '',
        loanPlan: '',
        dateTime: '',
        areaData: ''
      }
    };
  },
  components: {
    mPicker,
    mDatePicker,
    mAreaPicker
  },
  created() {
    this.slots = [{values: []}];
  },
  mounted() {},
  methods: {
    loanBreedEvent() {
      this.datakey = 'loanBreed';
      this.indexText = '态度';
      this.slots = [{values: [
                      {k: '01', v: '不错'},
                      {k: '02', v: '恩'},
                      {K: '03', v: '差'}
                  ]}];
      this.isPicker = true;
      this.isDatePicker = false;
      this.isAreaPicker = false;
    },
    loanPlanEvent() {
      this.isPicker = true;
      this.isDatePicker = false;
      this.isAreaPicker = false;
      this.datakey = 'loanPlan';
      this.indexText = '评论';
      this.slots = [{values: [
                      {k: '01', v: '好评'},
                      {k: '02', v: '一般'},
                      {K: '03', v: '差'}
                  ]}];
    },
    pickerConfirm(value, key) {
      this.isPicker = false;
      console.log(value, key);
      this.info[key] = value[this.valueKey];
    },
    pickerCancel() {
      this.isPicker = false;
    },
    // 日期选择器
    dateTimeSeleted() {
      this.dateDatakey = 'dateTime';
      this.isPicker = false;
      this.isDatePicker = true;
      this.isAreaPicker = false;
    },
    datePickerCancel() {
      this.isDatePicker = false;
    },
    datePickerConfirm(value, key) {
      this.isDatePicker = false;
      this.info[key] = value.join('-');
    },
    // 地区选择
    areaSeleted() {
      this.isPicker = false;
      this.isDatePicker = false;
      this.isAreaPicker = true;
    },
    areaPickerCancel() {
      this.isAreaPicker = false;
    },
    areaPickerConfirm(value, key) {
      this.isAreaPicker = false;
      this.info[key] = value.v;
    }
  }
};
</script>

<style scoped>
section{
  font-size: .14rem;
  margin-top: 1rem;
}
.cell-btn button{
  margin: 0 auto;
  margin-top: .5rem;
  width: 70%;
    background-color: #26a2ff;
  -webkit-appearance: none;
    -moz-appearance: none;
    appearance: none;
    border-radius: 4px;
    border: 0;
    box-sizing: border-box;
    color: inherit;
    display: block;
    height: .8rem;
  font-size: .28rem;
    outline: 0;
    overflow: hidden;
    position: relative;
    text-align: center;
  color: #fff;
}
</style>
