<template>
  <div id="app">
    <!-- <img alt="Vue logo" src="./assets/logo.png"> -->
    <!-- <HelloWorld msg="Welcome to Your Vue.js App"/> -->
    <JasSplit>
      <template #left>
        123
      </template>
    </JasSplit>
    <button @click="foo">点击的弹窗啊</button>
    <JasIframeDialog></JasIframeDialog>
  </div>
</template>

<script>
// import HelloWorld from './components/HelloWorld.vue'
import JasSplit from './components/split/index'
import './styles/index.scss'
import Vue from 'vue/dist/vue'
import JasIframeDialog from './components/jas-iframe-dialog/JasIframeDialog.vue'
var jasDialog = (function ( ) {

		var dialogs = [];
		/**
		 * @description 弹出弹出层
		 * @param params             参数对象
		 * @param params.id          参数对象
		 * @param params.title       参数对象
		 * @param params.src         参数对象
		 * @param params.height      参数对象
		 * @param params.width       参数对象
		 * @param params.cbForClose    模态框关闭的回调
		 */
		var show = function (params) {
			// var argument = params.argument;
			var obj =  {
				title: '模态框',
				src: 'https://www.awesomes.cn/',
				height: (80 - dialogs.length * 15) + '%',
				width: (80 - dialogs.length * 15) + '%',
				visible: true,
        ...params
			}

			var html = [
				'<jas-iframe-dialog',
				'  :title="title" ',
				'  :iframeUrl="iframeUrl" ',
				'  :height="height" ',
				'  :width="width" ',
				'  :visible.sync="visible" ',
				'  @close="close">',
				'</jas-iframe-dialog>'

			].join('');
      console.log(Vue);
      console.log(Vue.compile);
			var res = Vue.compile(html);
			var inst = new Vue({
				el: document.createElement('div'),
				data: {
					title: obj.title,
					iframeUrl: obj.src,
					height: obj.height,
					width: obj.width,
					visible: obj.visible,

				},

				methods: {
					close: function () {
						if (obj.cbForClose) {
							obj.cbForClose(this.paramForCallback);
						}
						var dom = this.$el;
						dom.parentNode.removeChild(dom); //删除
						dialogs.length = dialogs.length - 1;
						// console.log('关闭了一个dialogs')
						// this.$destroy();
						return;
					}
				},
				render: res.render,
				//staticRenderFns: res.staticRenderFns
			});
			document.body.appendChild(inst.$el);
			dialogs.push(inst);
		};
		/**
		 * @description 关闭最顶上的弹出层
		 * @param params             参数对象
		 */
		var close = function (param) {
			var index = dialogs.length - 1;
			if (index < 0) {
				console.log('没有可以关闭的dialogs')
				return;
			}
			var inst = dialogs[index];
			inst.paramForCallback = param;
			inst.visible = false;
		};
		return {
			// dialogs: dialogs,
			show: show,
			close: close
		};
	})( );
export default {
  name: 'App',
  components: {
    JasSplit,
    JasIframeDialog
  },
  created(){

  },
  methods:{
    foo(){
      jasDialog.show()
    }
  }
}
</script>

<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
