<template>
  <div>

  <el-dialog :title="title" :visible.sync="open" width="700px" append-to-body>
    <el-form ref="form" :model="form" :rules="rules" label-width="100px">
      <el-form-item label="上传测试文件" prop="uploadFile">
        <el-upload
          name="uploadFile"
        ref="upload"
        :data="form"
        :headers="headers"
        :auto-upload="false"
        :limit=limitNum
        accept=".png,.jpg,.pdf,.doc,.docx,.txt,.xls,.xlsx"
        :on-exceed="exceedFile"
        :on-change="handleChange"
        :on-remove="handleRemove"
        :on-success="handleSuccess"
        :on-error="handleError"
        :before-upload="beforeUploadFile"
        action="/dev-api/data/file/fileUpload"
        v-hasPermi="['data:file:fileUpload']"
        :file-list="fileList"
        multiple="true"
        show-file-list="true">
        <el-button size="small" type="primary">选择附件</el-button>
        </el-upload>
      </el-form-item>
    </el-form>
    <div slot="footer" class="dialog-footer">
      <el-button type="primary" @click="submitForm">上 传</el-button>
      <el-button @click="cancel">取 消</el-button>
    </div>
  </el-dialog>

  </div>
</template>

<script>
import { getToken } from './auth';
export default {
  name: "test",

  data() {
    return {
      headers:{
        "Authorization":'Bearer ' + getToken()
      },
      limitNum: 10,
      fileList:[],
      open:true,
      token:{
        accessToken:''
      },
    }
  },
  methods: {
    // 文件超出个数限制时的钩子
    exceedFile(files, fileList) {
      this.$message({
        message: '只能选择 ${this.limitNum} 个文件，当前共选择了 ${files.length + fileList.length} 个',
        type: 'danger'
      });
    },

    // 文件状态改变时的钩子
    handleChange(file, fileList) {
      this.filelist = fileList
    },
    // 文件移除时的钩子
    handleRemove(file, fileList) {
      this.filelist = fileList
    },

    // 上传文件之前的钩子, 参数为上传的文件,若返回 false 或者返回 Promise 且被 reject，则停止上传
    beforeUploadFile(file) {
      let  testmsg = file.name.substring(file.name.lastIndexOf('.')+1)
      const extension = testmsg === 'png'
      const extension2 = testmsg === 'jpg'
      const extension3 = testmsg === 'pdf'
      const extension4 = testmsg === 'doc'
      const extension5 = testmsg === 'docx'
      const extension6 = testmsg === 'txt'
      const extension7 = testmsg === 'xls'
      const extension8 = testmsg === 'xlsx'
      let size = file.size / 1024 / 1024
      if(!extension && !extension2 && !extension3 && !extension4 && !extension5 && !extension6 && !extension7 && !extension8) {
        this.$message({
          message: '上传文件只能是png、jpg、pdf、doc、docx、txt、xls、xlsx格式!',
          type: 'danger'
        });
      }
      if(size > 20) {
        this.$message({
          type: 'danger',
          message: `文件大小不得超过20M!`
        });
      }
      return extension || extension2 ||extension3 || extension4 ||extension5 || extension6 ||extension7 || extension8
    },

    // 文件上传成功时的钩子
    handleSuccess(res, file, fileList) {
      this.open = true;
      this.getList();
      this.$message({
        type: 'danger',
        message: `文件上传成功!`
      });
    },

    // 文件上传失败时的钩子
    handleError(err, file, fileList) {
      this.$message({
        type: 'danger',
        message: `文件上传失败!`
      });
    },

    /** 提交按钮 */
    submitForm: function() {
      this.$refs.upload.submit()

    },
  }



}




</script>

<style scoped>

</style>
