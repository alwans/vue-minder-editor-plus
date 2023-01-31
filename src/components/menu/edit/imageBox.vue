<template>
  <div class="upload-pic-group" :disabled="isDisabled">
    <el-dropdown trigger="click" szie="small">
      <span class="el-dropdown-link">
        <div><i class="el-icon-picture"></i></div>
        <div>图片<i class="el-icon-arrow-down el-icon--right"></i></div>
      </span>
      <el-dropdown-menu slot="dropdown">
        <el-dropdown-item class="el-dropdown-item">
          <el-upload
            class="avatar-uploader"
            accept=".jpg,.png,.jpeg"
            action=""
            :http-request="uploadImage"
            :show-file-list="false"
            :before-upload="beforeAvatarUpload"
            :disabled="isDisabled"
          >
            <span>插入图片</span>
          </el-upload>
        </el-dropdown-item>
        <el-dropdown-item class="el-dropdown-item">
          <div @click="removeImage" :disabled="isDisabled">移除已有图片</div>
        </el-dropdown-item
        >
      </el-dropdown-menu>
    </el-dropdown>
  </div>
</template>

<script>
import { imageProps } from "../../props";
import { isImageDisable, isDisableNode } from "../../../script/tool/utils";

export default {
  name: "imageBox",
  data() {
    return {
      minder: undefined,
    };
  },
  props: {
    ...imageProps,
  },
  mounted() {
    this.$nextTick(() => {
      this.minder = minder;
      // 点击节点，触发computed
    });
  },
  computed: {
    isDisabled() {
      return this.setIsDisabled();
    },
  },
  methods: {
    setIsDisabled() {
      try {
        if (!this.minder) return true;
      } catch (e) {
        // 如果window的还没挂载minder，先捕捉undefined异常
        return true;
      }
      if (!this.minder.getSelectedNode()) return true; //没有选择节点不可以上传
      if (isImageDisable(minder)) {
        return true;
      } else if (isDisableNode(this.minder)) {
        return true;
      }

      if (this.minder.queryCommandState) {
        return minder.queryCommandState("image") === -1;
      }
      if (this.minder.queryCommandState) {
        return minder.queryCommandState("imageSize") === -1;
      }
      return false;
    },
    beforeAvatarUpload(file) {
      const isJPG =
        file.type === "image/jpeg" ||
        file.type === "image/png" ||
        file.type === "image/jpg";
      const isLt2M = file.size / 1024 / 1024 < 2;

      if (!isJPG) {
        this.$message.error("上传头像图片只能是 JPG/PNG 格式!");
      }
      if (!isLt2M) {
        this.$message.error("上传头像图片大小不能超过 2MB!");
      }
      return isJPG && isLt2M;
    },
    uploadImage(file) {
      let selectNodes = minder.getSelectedNodes();
      if (selectNodes.length === 0) {
        this.$message.error("请选择节点");
        return;
      }
      const p = this.imageUpload(file);
      p.then((file) => {
        minder.execCommand("image", file.full_path_after_upload);
      });
    },
    removeImage(){
      let selectNodes = minder.getSelectedNodes();
      if (selectNodes.length === 0) {
        this.$message.error("请选择节点");
        return;
      }
      const p = this.imageDelete(selectNodes);
      p.then(() => {
        minder.execCommand("image", null);
      });
    }
  },
};
</script>

<style scoped>
.upload-pic-group {
  text-align: center;
  padding: 0 5px;
}
.el-dropdown-link,
.el-dropdown-item {
  font-size: 12px;
  color: #333;
}
.el-icon-picture {
  font-size: 18px;
}
</style>
