<template lang="html">
  <header>
    <el-tabs v-model="activeName" class="mind_tab-content">
      <el-tab-pane label="思维导图" name="editMenu">
        <div class="mind-tab-panel">
          <edit-menu
            :minder="minder"
            :move-enable="moveEnable"
            :sequence-enable="sequenceEnable"
            :tag-enable="tagEnable"
            :progress-enable="progressEnable"
            :image-enable="imageEnable"
            :priority-count="priorityCount"
            :priority-prefix="priorityPrefix"
            :tag-edit-check="tagEditCheck"
            :tag-disable-check="tagDisableCheck"
            :priority-disable-check="priorityDisableCheck"
            :priority-start-with-zero="priorityStartWithZero"
            :tags="tags"
            :distinct-tags="distinctTags"
            :image-upload="imageUpload"
            :image-delete="imageDelete"
          />
        </div>
      </el-tab-pane>
      <el-tab-pane label="外观样式" name="viewMenu">
        <div class="mind-tab-panel">
          <view-menu
            @moldChange="handleMoldChange"
            :minder="minder"
            :default-mold="defaultMold"/>
        </div>
      </el-tab-pane>
    </el-tabs>
  </header>
</template>

<script>
  import editMenu from '../menu/edit/editMenu'
  import viewMenu from '../menu/view/viewMenu'
  import {editMenuProps, moleProps, priorityProps, tagProps, imageProps} from "../props";
  export default {
    name: 'headerVue',
    data() {
      return {
        switchShow: {
          showEditMenu: true,
          showViewMenu: false,
        },
        activeName: 'editMenu'
      }
    },
    props: {
      ...editMenuProps,
      ...priorityProps,
      ...tagProps,
      ...moleProps,
      ...imageProps,
      minder: {}
    },
    components: {
      editMenu,
      viewMenu
    },
    methods: {
      handleMoldChange(data) {
        this.$emit('moldChange', data);
      },
      showMenu: function (e) {
        for (var variable in this.switchShow) {
          if (this.switchShow.hasOwnProperty(variable)) {
            this.switchShow[variable] = false
          }
        }
        this['switchShow'][e.target.className.replace('btn-', '')] = true
      }
    }
  }
</script>

<style lang="scss" >
  @import "src/style/header";
</style>

<style scoped>

  .mind_tab-content >>> .tab-icons {
    background-image: url("../../assets/minder/icons.png");
    background-repeat: no-repeat;
  }

  .el-tabs >>> .el-tabs__header {
    margin-bottom: 10px;
  }


</style>
