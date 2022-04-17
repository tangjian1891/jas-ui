<template>
  <el-dialog
    :ref="refSelf"
    class="jas-iframe-dialog"
    :close-on-click-modal="false"
    :title="title"
    :visible.sync="selfvisible"
    :width="width"
    @close="close"
    :fullscreen="false"
  >
    <!-- <iframe class="dialog-iframe" :src="iframeUrl" frameborder="0"></iframe> -->
    这是百度啊
  </el-dialog>
</template>

<script>
import { Dialog } from "element-ui";
console.log(Dialog, "这是是");
export default {
  name: "JasIframeDialog",
  components: {
    [Dialog.name]: Dialog,
  },
  props: {
    refSelf: {
      default: "jas-iframe-dialog",
      type: String,
    },
    width: {
      default: "80%",
      type: String,
    },
    height: {
      default: "80%",
      type: String,
    },
    iframeUrl: {
      default: "",
      type: String,
    },
    visible: {
      default: false,
      type: Boolean,
    },
    title: {
      default: "提示",
      type: String,
    },
  },
  data: function () {
    return {
      selfvisible: true,
      // name: "你好啊啊",
    };
  },
  watch: {
    visible: function () {
      this.selfvisible = this.visible;
    },
    selfvisible: function (newValue) {
      this.$emit("update:visible", newValue);
    },
  },
  methods: {
    close: function () {
      // this.$emit('update:visible', false)
      this.$emit("close");
    },
    setDialogHeiht: function () {
      var dom = this.$el.querySelector(".el-dialog");
      console.log(dom);
      if (this.height.indexOf("%") !== -1) {
        var height = this.height.split("%")[0];
        if (height <= 0 || height >= 100) return {};
        dom.style["margin-top"] = (100 - height) / 2 + "vh";
        dom.style["height"] = height + "vh";
      } else if (this.height.indexOf("px") !== -1) {
        var height02 = this.height.split("px")[0];
        var wrap_height = document.documentElement.clientHeight;
        if (height02 < wrap_height) {
          var toper = (wrap_height - height02) / 2;
          dom.style["margin-top"] = toper + "px";
        }
        dom.style["height"] = this.height;
      }
    },
  },
  mounted: function () {
    this.setDialogHeiht();
  },
};
</script>

<style></style>
