<template>
  <Layout class-prefix="layout">
    <NumberPad :value.sync="record.amount" @submit="saveRecord"/>
    <Tabs :data-source="recordTypeList" :value.sync="record.type"/>
    <div class="notes">
      <FromItem file-name="备注"  :value.sync="record.notes" placeholder="在这里输入备注"/>
    </div>
    <Tags @update:value="toggle"/>
  </Layout>
</template>

<script lang="ts">
import Vue from 'vue';
import NumberPad from '@/components/Money/NumberPad.vue';
import FromItem from '@/components/Money/FromItem.vue';
import Tags from '@/components/Money/Tags.vue';
import {Component} from 'vue-property-decorator';
import recordTypeList from '@/constant/recordTypeList';
import Tabs from '@/components/Tabs.vue';

@Component({
  components:{Tabs, Tags,FromItem,NumberPad},

})


export default class Money extends Vue {
  get recordList() {
    return this.$store.state.recordList;
  }

  recordTypeList=recordTypeList

  record: RecordItem = {tags: [], notes: '', type: '-', amount: 0};
  created(){
    this.$store.commit('fetchRecords')
  }

  onUpdateNotes(value: string) {
    this.record.notes = value;
  }

  toggle(tag:[]) {
    this.record.tags = tag;
  }
  onUpdateAmount(value: string) {
    this.record.amount = parseFloat(value);
  }


  saveRecord() {
    if (!this.record.tags || this.record.tags.length === 0){
      return window.alert('请至少选择一个标签')
    }
    this.$store.commit('createRecord',this.record);
    if (this.$store.state.createRecordError === null){
      window.alert('已保存')
      this.record.notes = ''
    }
  }
};
</script>

<style lang="scss" >
.layout-content {
  display: flex;
  flex-direction: column-reverse;
}

.notes {
  padding: 12px 0;
}
</style>
