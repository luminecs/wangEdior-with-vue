<template>
  <div class="home">
    <h3>wangEditor with vue</h3>
    <div id="demo1" ref="myEditor"></div>
    <button type="button" class="btn" @click="getEditorData">获取当前内容</button>
    <h3>内容预览</h3>
    <textarea name="" id="" cols="170" rows="20" readonly v-model="editorData"></textarea>
  </div>
</template>

<script>

// 引入 wangEditor
import WangEditor from 'wangeditor'

export default {
  data () {
    return {
      editor: null,
      editorData: ''
    }
  },
  mounted () {
    const editor = new WangEditor(this.$refs.myEditor)

    // 配置 onchange 回调函数，将数据同步到 vue 中
    editor.config.onchange = (newHtml) => {
      this.editorData = newHtml
    }

    // 设置编辑区域高度为 500px
    editor.config.height = 100

    // 创建编辑器
    editor.create()

    // editor.txt.append('<p>追加的内容</p>')
    this.editor = editor
  },
  methods: {
    getEditorData () {
      // 通过代码获取编辑器内容
      const data = this.editor.txt.html()
      console.info(data)
      const textData = this.editor.txt.text()
      console.info(textData)
      const json = this.editor.txt.getJSON()
      console.info(json)
      this.editor.txt.clear()
    }
  },
  beforeDestroy () {
    // 调用销毁 API 对当前编辑器实例进行销毁
    this.editor.destroy()
    this.editor = null
  }
}
</script>

<style lang="scss">
  .home {
    width: 1200px;
    margin: auto;
    position: relative;
    .btn {
      position: absolute;
      right: 0;
      top: 0;
      padding: 5px 10px;
      cursor: pointer;
    }
    h3 {
      margin: 30px 0 15px;
    }
  }
</style>
