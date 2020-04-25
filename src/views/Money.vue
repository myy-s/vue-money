<template>
  <Layout class-prefix="layout">
    {{recordList}}
    <NumberPad @update:value="onUpdateAmount" @submit="saveRecord" />
    <Types :value.sync="record.type" />
    <Notes @update:value="onUpdateNotes" />
    <Tags :data-source.sync="tags" @update:value="onUpdateTages" />
  </Layout>
</template>

<script lang="ts">
import NumberPad from "@/components/Money/NumberPad.vue";
import Types from "@/components/Money/Types.vue";
import Notes from "@/components/Money/Notes.vue";
import Tags from "@/components/Money/Tags.vue";
import Vue from "vue";
import { Component, Watch } from "vue-property-decorator";
import model from "@/model";

const recordList = model.fetch();

@Component({
  components: { Tags, Notes, Types, NumberPad }
})
export default class Money extends Vue {
  tags = ["衣", "食", "住", "行"];
  recordList: RecordItem[] = recordList;
  record: RecordItem = {
    tags: [],
    notes: "",
    type: "-",
    amount: 0
  };
  onUpdateAmount(value: string) {
    this.record.amount = parseFloat(value);
  }

  onUpdateNotes(value: string) {
    this.record.notes = value;
  }

  onUpdateTages(value: string[]) {
    this.record.tags = value;
  }

  saveRecord() {
    const record: RecordItem = model.clone(this.record);
    record.createdAt = new Date();
    this.recordList.push(record);
  }

  @Watch("recordList")
  onRecordListChanged() {
    model.save(this.recordList);
  }
}
</script>
<style lang="scss">
.layout-content {
  display: flex;
  flex-direction: column-reverse;
}
</style>
