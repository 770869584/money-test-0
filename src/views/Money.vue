<template>
  <Layout class-prefix="layout">
    <NumberPad :value.sync="record.amount" @submit="saveRecord"/>

    <Types :value.sync="record.type"/>
    <div class="notes">
      <FromItem file-name="备注" @update:value="onUpdateNotes" placeholder="在这里输入备注"/>
    </div>
    <Tags :data-source.sync="tags" @update:value="onUpdateTags"/>
  </Layout>
</template>

<script lang="ts">
import Vue from 'vue';
import NumberPad from '@/components/Money/NumberPad.vue';
import FromItem from '@/components/Money/FromItem.vue';
import Types from '@/components/Money/Types.vue';
import Tags from '@/components/Money/Tags.vue';
import Component from 'vue-class-component';
import store from '@/store/index2';

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

@Component({
  components: {Tags, Types, FromItem, NumberPad},
})
export default class Money extends Vue {
  tags = store.tagList;
  record: RecordItem = {tags: [], notes: '', type: '-', amount: 0};
  recordList = store.recordList;

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
   store.createRecord(this.record);
  }
};
</script>

<style lang="scss">
.layout-content {
  display: flex;
  flex-direction: column-reverse;
}

.notes {
  padding: 12px 0;
}
</style>
