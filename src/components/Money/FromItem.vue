<template>
  <div>
    <label class="fromItem">
      <span class="name">{{ this.fileName }}</span>
      <input type="text"
             :value="value"
             @input="onValueChanged($event.target.value)"
             :placeholder="this.placeholder">
    </label>
  </div>

</template>

<script lang="ts">
  import Vue from 'vue';
  import {Component, Prop, Watch} from 'vue-property-decorator';

  @Component
  export default class FromItem extends Vue{
    @Prop({default:''}) readonly value!:string;

    @Prop({required:true})fileName!:string

    @Prop()placeholder?:string
    @Watch('value')
    onValueChanged(value:string){
      this.$emit('update:value',value)
    }
};
</script>

<style scoped lang="scss">
@import "../../assets/style/helper";

.fromItem {
  font-size: 14px;
  padding-left: 16px;
  display: flex;
  align-items: center;

  .name {
    padding-right: 16px;
  }

  input {
    height: 44px;
    flex-grow: 1;
    background: transparent;
    border: none;
  }
}
</style>