<template>
<div :disabled="commandDisabled">
    <el-tag v-for="(item, index) in tags"
            :key="item"
            size="small"
            :color="getResourceColor(item)"
            @click="editResource(item)">{{item}}</el-tag>
</div>
</template>

<script>

import {tagProps} from "../../props";
import {isDisableNode, isTagEnable} from "../../../script/tool/utils";

export default {
  name: 'tagBox',
  props: {
    ...tagProps
  },
  data() {
    return {
      minder: undefined
    }
  },
  computed: {
    commandDisabled() {
      let minder = this.minder;
      if (!minder) return true;
      minder.on && minder.on('interactchange', function () {
        this.commandValue = minder.queryCommandValue('resource');
      });
      let node = minder.getSelectedNode();
      if (node && node.data.allowDisabledTag) {
        return false;
      }
      if (isDisableNode(minder) && !isTagEnable(minder)) {
        return true;
      }
      if (this.tagDisableCheck) {
        return this.tagDisableCheck();
      }
      return minder.queryCommandState && minder.queryCommandState('resource') === -1;
    },
  },
  mounted() {
    this.$nextTick(() => {
      this.minder = minder;
    })
  },
  methods: {
    getResourceColor(resource) {
      if (this.minder && this.minder.getResourceColor) {
        if (resource == "通过" || resource == "Pass" || resource == "通過"){
          return "#3CBD28"
        }
        if (resource == "不通过" || resource == "UnPass" || resource == "不通過"){
          return "#FF6666"
        }
        if (resource == "备注" || resource == "Remark" || resource == "備註"){
          return "#E6A23D"
        }
        if (resource == "失败" || resource == "Failure" || resource == "失敗"){
          return "#DC143C"
        }
        if (resource == "阻塞" || resource == "Blocking" || resource == "阻塞"){
          return "#1AABE0"
        }
        if (resource == "跳过" || resource == "Skip" || resource == "跳過"){
          return "#C57AD6"
        }
        if (resource == "用例" || resource == "Case" || resource == "用例"){
          return "#FFD900"
        }
        if (resource == "前置条件" || resource == "Prerequisite" || resource == "前置條件"){
          return "#DA71D7"
        }
        if (resource == "标签" || resource == "Tag" || resource == "標簽"){
          return "#B1E0E7"
        }
        return this.minder.getResourceColor(resource).toHEX();
      }
    },
    editResource(resourceName) {
      if (this.commandDisabled) {
        return;
      }
      // if (this.tagEditCheck) {
      //   if (!this.tagEditCheck(resourceName)) {
      //     return;
      //   }
      // }
      let origin = this.minder.queryCommandValue('resource');
      if (!resourceName || !/\S/.test(resourceName)) return;
      let index = origin.indexOf(resourceName);
      // 先删除排他的标签
      if (this.distinctTags.indexOf(resourceName) > -1) {
        for (let i = 0; i < origin.length; i++) {
          if (this.distinctTags.indexOf(origin[i]) > -1) {
            origin.splice(i, 1);
            i--;
          }
        }
      }
      if (index != -1) {
        origin.splice(index, 1);
      } else {
        origin.push(resourceName);
      }
      this.minder.execCommand('resource', origin);
    },
  },
}
</script>

<style scoped>

 .el-tag {
   margin-right: 4px;
   border: 0px;
   color: black;
 }

 .el-tag:hover {
   cursor: pointer;
 }

 .el-tag:first-child {
   margin-left: 4px;
 }
</style>

<style scoped>
   .add-btn {
     height: 24px;
     width: 36px;
     padding: 0px !important;
     border-style: dashed !important;
   }
</style>
