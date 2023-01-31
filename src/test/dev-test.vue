<template>
  <div>
    <minder-editor
      :import-json="importJson"
      :progress-enable="true"
      :tag-enable="true"
      :sequence-enable="true"
      :image-enable="true"
      :tags="tags"
      :move-enable="true"
      :disabled="true"
      :distinct-tags="tags"
      :height="500"
      :priority-count="4"
      :tag-edit-check="test"
      :priority-disable-check="test"
      @afterMount="afterMount()"
      :default-mold="3"
      @moldChange="handleMoldChange"
      @save="save"
      :image-upload="imageUpload"
      :image-delete="imageDelete"
    />
  </div>
</template>

<script>
import Vue from 'vue'
import minderEditor from "../components/plugin";
Vue.use(minderEditor) //开发测试

export default {
  name: "dev-test",
  data() {
    return {
      importJson: {
        "root": {
          "data": {
            "text": "test222",
            // "disable": true
          },
          "children": [
            {
              "data": { "text": "地图axxaaaa",
                disable: true,
                expandState: "collapse",
                // tagEnable: true,
                // allowDisabledTag: true,
                resource: ["模块1"]},
              "children": [
                {
                  "data": { "text": "地图axxaaaa",
                    allowDelete: true,
                    allowImage: false,
                    // tagEnable: true,
                    // allowDisabledTag: true,
                    resource: ["模块12"]}
                },
                {
                  "data": { "text": "111"}
                },
                {
                  "data": { "text": "222"}
                },
                {
                  "data": { "text": "333"}
                },
                ]
            },
            { "data": {
              "text": "百科aa",
                "expandState":"collapse"}}
          ]
        },
        "template":"default"
      },
      tags:  ['模块1','用例']
    }
  },
  mounted() {

  },

  methods: {
    save(data) {
    },
    test() {
      // return () => {
        return false
      // };
    },
    handleMoldChange(a) {
      // console.log(a);
    },
    afterMount() {
      // minder.on('selectionchange ', function (env) {
      //   console.log('selectionchange');
      //   console.log(env);
      //   // let selectNodes = env.minder.getSelectedNodes();
      //   // if (selectNodes) {
      //   //   selectNodes.forEach((node) => {
      //   //     markChangeNode(node);
      //   //   })
      //   // }
      // });
      minder.on('contentchange', function (env) {
        // console.log(env);
        let selectNodes = env.minder.getSelectedNodes();

      });
      // minder.on('afterExecCommand', function (env) {
      //   console.log('afterExecCommand');
      //   console.log(env);
      // });
      // minder.on('preExecCommand', function (env) {
      //   console.log('preExecCommand');
      //   console.log(env);
      // });
      minder.on('beforeExecCommand', function (env) {
      });

      this.addHotBox();

    },
    addHotBox() {

      let hotbox = window.minder.hotbox;
      let main = hotbox.state('main');
      main.button({
        position: 'ring',
        label: 'Test',
        key: '/',
        action: function () {
          alert("test")
        },
        enable: function () {
          return true;
        },
        beforeShow: function () {
        }
      })
    },
    imageUpload(file) {
      file.full_path_after_upload = "https://ysg.mihoyo.com:8080/casehost/resource/md/get/717a7570_%E6%88%AA%E5%B1%8F2022-12-05%2014.44.13.png"
      return new Promise((resolve, reject) => {
        setTimeout(() => {
          resolve(file)
        }, 1000);
      })
    },
    imageDelete(nodes) {
      console.log(nodes)
    }
  }
}
</script>

<style scoped>

</style>
