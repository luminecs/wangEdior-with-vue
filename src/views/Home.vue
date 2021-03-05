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

    // 上传图片
    // 配置 server 接口地址
    editor.config.uploadImgServer = '/upload-img'
    // 限制图片大小2M 默认5M
    editor.config.uploadImgMaxSize = 2 * 1024 * 1024
    // 限制图片类型
    editor.config.uploadImgAccept = ['jpg', 'jpeg', 'png', 'gif', 'bmp']
    // 限制一次最多能传几张图片 默认为 100 张 一次最多上传 5 个图片
    editor.config.uploadImgMaxLength = 5
    // 自定义上传参数
    editor.config.uploadImgParams = {
      token: 'xxxxx',
      x: 100
    }
    // 自定义 fileName
    editor.config.uploadFileName = 'your-custom-fileName'
    // 自定义 header
    editor.config.uploadImgHeaders = {
      Accept: 'text/x-json',
      a: 100
    }
    // withCredentials（跨域传递 cookie）
    editor.config.withCredentials = true
    // 自定义 timeout 时间 默认是 10 秒钟
    editor.config.uploadImgTimeout = 5 * 1000
    // 回调函数
    editor.config.uploadImgHooks = {
      // 上传图片之前
      before: function (xhr) {
        console.log(xhr)

        // 可阻止图片上传
        return {
          prevent: true,
          msg: '需要提示给用户的错误信息'
        }
      },
      // 图片上传并返回了结果，图片插入已成功
      success: function (xhr) {
        console.log('success', xhr)
      },
      // 图片上传并返回了结果，但图片插入时出错了
      fail: function (xhr, editor, resData) {
        console.log('fail', resData)
      },
      // 上传图片出错，一般为 http 请求的错误
      error: function (xhr, editor, resData) {
        console.log('error', xhr, resData)
      },
      // 上传图片超时
      timeout: function (xhr) {
        console.log('timeout')
      },
      // 图片上传并返回了结果，想要自己把图片插入到编辑器中
      // 例如服务器端返回的不是 { errno: 0, data: [...] } 这种格式，可使用 customInsert
      customInsert: function (insertImgFn, result) {
        // result 即服务端返回的接口
        console.log('customInsert', result)

        // insertImgFn 可把图片插入到编辑器，传入图片 src ，执行函数即可
        insertImgFn(result.data[0])
      }
    }
    // 自己实现上传图片
    editor.config.customUploadImg = function (resultFiles, insertImgFn) {
      // resultFiles 是 input 中选中的文件列表
      // insertImgFn 是获取图片 url 后，插入到编辑器的方法
      const imgUrl = ''
      // 上传图片，返回结果，将图片插入到编辑器中
      insertImgFn(imgUrl)
    }
    // 隐藏插入网络图片的功能
    // editor.config.showLinkImg = false
    // 网络图片配置alt选项
    editor.config.showLinkImgAlt = false
    // 网络图片配置超链接
    editor.config.showLinkImgHref = false
    // 插入网络图片的回调
    editor.config.linkImgCallback = function (src, alt, href) {
      console.log('图片 src ', src)
      console.log('图片文字说明', alt)
      console.log('跳转链接', href)
    }

    // 上传视频
    // 配置 server 接口地址
    editor.config.uploadVideoServer = '/api/upload-video'
    // 限制视频大小 默认限制视频大小是 1024m
    editor.config.uploadVideoMaxSize = 1 * 1024 * 1024 * 1024 // 1024m
    // 限制类型
    editor.config.uploadVideoAccept = ['mp4']
    // 自定义上传参数
    editor.config.uploadVideoParams = {
      token: 'xxxxx',
      x: 100
    }
    // 如果需要将参数拼接到 url 中，可再加上如下配置。
    editor.config.uploadVideoParamsWithUrl = true
    // 自定义 fileName
    editor.config.uploadVideoName = 'your-custom-fileName'
    // 自定义 header
    editor.config.uploadVideoHeaders = {
      Accept: 'text/x-json',
      a: 100
    }
    // withCredentials（跨域传递 cookie）
    editor.config.withVideoCredentials = true
    // 自定义 timeout 时间 默认是 5分钟
    editor.config.uploadVideoTimeout = 1000 * 60 * 5
    // 回调函数
    editor.config.uploadVideoHooks = {
      // 上传视频之前
      before: function (xhr) {
        console.log(xhr)

        // 可阻止视频上传
        return {
          prevent: true,
          msg: '需要提示给用户的错误信息'
        }
      },
      // 视频上传并返回了结果，视频插入已成功
      success: function (xhr) {
        console.log('success', xhr)
      },
      // 视频上传并返回了结果，但视频插入时出错了
      fail: function (xhr, editor, resData) {
        console.log('fail', resData)
      },
      // 上传视频出错，一般为 http 请求的错误
      error: function (xhr, editor, resData) {
        console.log('error', xhr, resData)
      },
      // 上传视频超时
      timeout: function (xhr) {
        console.log('timeout')
      },
      // 视频上传并返回了结果，想要自己把视频插入到编辑器中
      // 例如服务器端返回的不是 { errno: 0, data: { url : '.....'} } 这种格式，可使用 customInsert
      customInsert: function (insertVideoFn, result) {
        // result 即服务端返回的接口
        console.log('customInsert', result)

        // insertVideoFn 可把视频插入到编辑器，传入视频 src ，执行函数即可
        insertVideoFn(result.data.url)
      }
    }
    // 自定义上传视频
    // PS：配置自定义插入视频 editor.config.customInsertVideo 也可以在这里起作用。
    editor.config.customUploadVideo = function (resultFiles, insertVideoFn) {
      // resultFiles 是 input 中选中的文件列表
      // insertVideoFn 是获取视频 url 后，插入到编辑器的方法
      const videoUrl = ''
      // 上传视频，返回结果，将视频地址插入到编辑器中
      insertVideoFn(videoUrl)
    }
    // 隐藏插入网络视频的功能
    editor.config.showLinkVideo = false
    // 自定义插入视频
    editor.config.customInsertVideo = function (videoUrl) {
      // videoUrl 是返回的视频地址
      // 往编辑器插入 html 内容
      editor.cmd.do(
        'insertHTML',
        `<p>
            <p contenteditable=false id="mse" style="max-width:100%"></p>
        </p>`
      )
      // 初始化视频
      // new Player({
      //   id: 'mse',
      //   url: videoUrl
      // })
    }
    // <iframe width="560" height="315" src="https://www.youtube.com/embed/dyZDkPTtCqg" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
    // 自定义检查插入视频的回调
    editor.config.onlineVideoCallback = function (video) {
      // 自定义回调内容，内容成功插入后会执行该函数
      console.log('插入视频内容', video)
    }
    // 自定义检查插入视频的链接
    editor.config.onlineVideoCheck = function (video) {
      // 编辑器会根据返回的内容做校验：比如以下几种情况

      // 1. 返回 true ，说明检查通过
      return true

      // 2. 返回一个字符串，说明检查未通过，编辑器会阻止视频插入。会 alert 出错误信息（即返回的字符串）
      // return '插入的视频 有 xxx 错误'

      // 3. 返回 undefined（即没有任何返回），说明检查未通过，编辑器会阻止视频插入。
      // 此处，你可以自定义提示错误信息，自由发挥
    }

    // 自定义检查插入的链接
    editor.config.linkCheck = function (text, link) {
      // 以下情况，请三选一

      // 1. 返回 true ，说明检查通过
      return true

      // // 2. 返回一个字符串，说明检查未通过，编辑器会阻止链接插入。会 alert 出错误信息（即返回的字符串）
      // return '链接有 xxx 错误'

      // 3. 返回 undefined（即没有任何返回），说明检查未通过，编辑器会阻止链接插入。
      // 此处，你可以自定义提示错误信息，自由发挥
    }

    // 自定义检查插入图片的链接
    // 参数中的imgSrc、alt、href分别代表图片地址、图片文本说明和跳转链接
    // 后面两个参数是可选参数
    editor.config.linkImgCheck = function (imgSrc, alt, href) {
      // 以下情况，请三选一

      // 1. 返回 true ，说明检查通过
      return true

      // // 2. 返回一个字符串，说明检查未通过，编辑器会阻止图片插入。会 alert 出错误信息（即返回的字符串）
      // return '图片 src 有 xxx 错误'

      // 3. 返回 undefined（即没有任何返回），说明检查未通过，编辑器会阻止图片插入。
      // 此处，你可以自定义提示错误信息，自由发挥
    }

    // 关闭粘贴样式的过滤
    editor.config.pasteFilterStyle = false
    // 忽略粘贴内容中的图片
    editor.config.pasteIgnoreImg = true
    // 自定义处理粘贴的文本内容
    // 配置粘贴文本的内容处理
    editor.config.pasteTextHandle = function (pasteStr) {
      // 对粘贴的文本进行处理，然后返回处理后的结果
      return pasteStr + '巴拉巴拉'
    }
    // 设置编辑区域高度为 500px
    editor.config.height = 100
    // 创建编辑器
    editor.create()
    // editor.txt.append('<p>追加的内容</p>')
    this.editor = editor

    // 常用API
    // editor 属性
    // console.info('编辑器唯一的 id', editor.id)
    // console.info('编辑器的所有配置', editor.config)
    // console.info('编辑区域 DOM 节点', editor.$textElem.elems[0])
    // console.info('元素 id', editor.textElemId)
    // console.info('菜单栏 DOM 节点', editor.$toolbarElem.elems[0])
    // console.info('元素 id', editor.toolbarElemId)
    // 选区范围 API
    // console.info('选中的文字', editor.selection.getSelectionText())
    // console.info('选区所在的 DOM 节点', editor.selection.getSelectionContainerElem().elems[0])
    // console.info('选区开始的 DOM 节点', editor.selection.getSelectionStartElem().elems[0])
    // console.info('选区结束的 DOM 节点', editor.selection.getSelectionEndElem().elems[0])
    // console.info('折叠选区', editor.selection.collapseRange())
    // console.info('判断选区是否为“空”（即没有选中任何文字）', editor.selection.isSelectionEmpty())
    // 内容操作 API
    // console.info('在光标位置插入文字', editor.cmd.do('insertHTML', '<p>...</p>'))

    // 禁用编辑器
    // editor.disable()
    // editor.enable()
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
