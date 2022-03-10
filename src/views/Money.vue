<template>
  <Layout class-prefix="layout">
    {{ recordList }}
    <NumberPad :value.sync="record.amount" @submit="saveRecord"/>

    <Types :value.sync="record.type"/>
    <Notes file-name="备注" @update:value="onUpdateNotes" placeholder="在这里输入备注"/>
    <Tags :data-source.sync="tags" @update:value="onUpdateTags"/>
  </Layout>
</template>

<script lang="ts">
import Vue from 'vue';
import NumberPad from '@/components/Money/NumberPad.vue';
import Notes from '@/components/Money/Notes.vue';
import Types from '@/components/Money/Types.vue';
import Tags from '@/components/Money/Tags.vue';
import Component from 'vue-class-component';
import {Watch} from 'vue-property-decorator';
import recordListModel from '@/models/recordListModel';
import tagListModel from '@/models/tagListModel';

// const model = require('@/model.js').default;
// // const version = window.localStorage.getItem('version') || 0;
// const recordList: RecordItem[] = model.fetch();
// if (version === '0.0.1') {
//   recordList.forEach(record => {record.createdAt = new Date(2000, 0, 0);});
//   //保存数据
//   window.localStorage.setItem('recordList', JSON.stringify(recordList));
//
// }
// window.localStorage.setItem('version', '0.0.1');
const recordList = recordListModel.fetch();
const tagList = tagListModel.fetch();

@Component({
  components: {Tags, Types, Notes, NumberPad},
})
export default class Money extends Vue {
  tags = tagList;
  record: RecordItem = {tags: [], notes: '', type: '-', amount: 0};
  recordList: RecordItem[] = recordList;

  onUpdateTags(value: string[]) {
    this.record.tags = value;
  }

  onUpdateNotes(value: string) {
    this.record.notes = value;
  }

  onUpdateAmount(value: string) {
    this.record.amount = parseFloat(value);
  }

  saveRecord() {
    const record2: RecordItem = recordListModel.clone(this.record);
    record2.createdAt = new Date();
    this.recordList.push(record2);
  }

  @Watch('recordList')
  onRecordListChange() {
    recordListModel.save(this.recordList)
  }
};
</script>

<style lang="scss">
.layout-content {
  display: flex;
  flex-direction: column-reverse;
}
</style>

<style lang="scss" scoped>

</style>