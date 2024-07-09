<template>
  <div class="upload-container">
    <el-button :style="{ background: color, borderColor: color }" icon="el-icon-upload" size="mini" type="primary" @click="dialogVisible = true">
      素材上传
    </el-button>

    <el-dialog v-model="dialogVisible">
      <el-upload v-model:file-list="fileList" :multiple="true" :before-upload="beforeUpload" class="editor-slide-upload" list-type="picture-card">
        <el-button size="small" type="primary"> 点击上传 </el-button>
      </el-upload>
      <el-button @click="dialogVisible = false"> 取消 </el-button>
      <el-button type="primary" @click="handleSubmit"> 确定 </el-button>
    </el-dialog>
  </div>
</template>

<script>
import { upload } from '@/utils/upload.js'

export default {
  name: 'EditorSlideUpload',
  props: {
    color: {
      type: String,
      default: '#1890ff'
    }
  },
  data() {
    return {
      dialogVisible: false,
      fileList: [],
      files: []
    }
  },
  methods: {
    handleSubmit() {
      console.log("successCBK, handleSubmit", this.files)
      this.$emit('successCBK', this.files)
      this.fileList = []
      this.files = []
      this.dialogVisible = false
    },
    beforeUpload(file) {
      const _this = this
      console.log("beforeUpload", file)
      upload(file).then(res => {
        const reg = new RegExp(/(image)/ig)
        if (reg.test(res.mimeType)) {
          res.cover = res.url
        }
        // if (res.mimeType === 'image/png' || res.mimeType === 'image/jpg') res.cover = res.url
        
        _this.fileList.push({ url: res.cover })
        _this.files.push(res)
      })
    }
  }
}
</script>

<style lang="scss" scoped>
.editor-slide-upload {
  margin-bottom: 20px;
  :deep(.el-upload-list--picture-card) {
    width: 100%;

    .el-upload--picture-card {
      width: 100%;
    }

    .el-upload-list__item-preview {
      display: none;
    }
  }
}
</style>
