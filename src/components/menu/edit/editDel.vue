<template>
<div class="edit-del-group">
  <div class="edit menu-btn" :disabled="textDisabled" @click="edit" title="编辑">
    <i class="tab-icons"></i>
    <span>编辑</span>
  </div>
  <div class="del menu-btn" :disabled="removeNodeDisabled" @click="del" title="删除">
    <i class="tab-icons"></i>
    <span>删除</span>
  </div>
</div>
</template>

<script>
import {isDeleteDisableNode, isDisableNode, markDeleteNode} from "../../../script/tool/utils";
export default {
  name: 'edit_del',
  data() {
    return {
      minder: undefined
    }
  },
  mounted() {
    this.$nextTick(() => {
      this.minder = minder;
      // 点击节点，触发computed
    })
  },
  computed: {
    textDisabled() {
      return this.isDisabled('text');
    },
    removeNodeDisabled() {
      return this.isDisabled('RemoveNode');
    },
  },
  methods: {
    isDisabled(command) {
      try {
        if (!this.minder) return false;
      } catch (e) {
        // 如果window的还没挂载minder，先捕捉undefined异常
        return false
      }
      if (command === 'RemoveNode') {
        if (isDeleteDisableNode(minder)) return true;
      } else if (isDisableNode(minder)) {
        return true;
      }
      if (minder) {
        return minder.queryCommandState && minder.queryCommandState(command) === -1
      }
    },
    edit() {
      if (this.textDisabled) {
        return;
      }
      minder.queryCommandState('text') === -1 || this.editNode();
    },
    del() {
      if (this.removeNodeDisabled) {
        return;
      }
      markDeleteNode(this.minder);
      minder.queryCommandState('RemoveNode') === -1 || minder.execCommand('RemoveNode');
    },
    editNode() {
      let editor = minderEditor;
      let receiverElement = editor.receiver.element;
      let fsm = editor.fsm;
      let receiver = editor.receiver;

      receiverElement.innerText = minder.queryCommandValue('text');
      fsm.jump('input', 'input-request');
      receiver.selectAll();
    }
  }
}
</script>
