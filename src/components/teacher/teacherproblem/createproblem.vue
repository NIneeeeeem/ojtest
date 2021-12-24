<template>
  <div>
    <el-col :span="18">
      <el-card>
        <el-form ref="form2" :model="form" label-width="150px">
          <el-form-item label="problemid">
            <el-col :span="5">
              <el-input v-model="problemid" placeholder="problemid" clearable></el-input>
            </el-col>
          </el-form-item>
          <el-form-item label="problemname">
            <el-col :span="5">
              <el-input v-model="problemname" placeholder="problemname" clearable></el-input>
            </el-col>
          </el-form-item>
          <el-form-item label="input">
            <el-col :span="5">
              <el-input v-model="input" placeholder="input"  clearable></el-input>
            </el-col>
          </el-form-item>
          <el-form-item label="output">
            <el-col :span="5">
              <el-input v-model="output" placeholder="output" clearable></el-input>
            </el-col>
          </el-form-item>


          <el-form-item>
            <el-button type="primary" @click="addtag">add tag</el-button>
            <el-button type="primary" @click="addsimple">add simple input and output</el-button>
            <el-button type="primary" @click="addinandout">add test input and output</el-button>
          </el-form-item>



<!--          <el-form-item label="sampleinput">-->
<!--            <el-col :span="5">-->
<!--              <el-input v-model="sampleinput1" placeholder="sample1"></el-input>-->
<!--            </el-col>-->
<!--            <el-col class="line" :span="1">-</el-col>-->
<!--            <el-col :span="5">-->
<!--              <el-input v-model="sampleinput2" placeholder="sample2"></el-input>-->
<!--            </el-col>-->
<!--          </el-form-item>-->
<!--          <el-form-item label="sampleoutput">-->
<!--            <el-col :span="5">-->
<!--              <el-input v-model="sampleoutput1" placeholder="sample1"></el-input>-->
<!--            </el-col>-->
<!--            <el-col class="line" :span="1">-</el-col>-->
<!--            <el-col :span="5">-->
<!--              <el-input v-model="sampleoutput2" placeholder="sample2"></el-input>-->
<!--            </el-col>-->
<!--          </el-form-item>-->
          <el-form-item label="Description:">
            <el-input type="textarea" :rows="10" placeholder="description" v-model="description" clearable></el-input>
            <el-divider></el-divider>
          </el-form-item>
          <el-form-item label="Hints:">
            <el-input type="textarea" :rows="5" placeholder="hints" v-model="hints" clearable></el-input>

            <el-divider></el-divider>
          </el-form-item>

             <el-col class="line" :span="7">-</el-col>
            <el-button type="primary" @click="onSubmit">add</el-button>

            <el-button>cancel</el-button>
        </el-form>
      </el-card>
    </el-col>


    <el-dialog title="添加标签" :visible="givetag" size="tiny" :modal-append-to-body='false' @close='closeDialog'>
      <el-form ref="atag" :model="atag" label-width="80px">
        <el-form-item label=tag: prop="tagg">
          <el-select v-model="atag.tag"
                     placeholder="选择标签">
            <el-option label="tag1" value="tag1" ></el-option>
            <el-option label="tag2" value="tag2"></el-option>
            <el-option label="tag3" value="tag3"></el-option>
            <el-option label="tag3" value="tag4"></el-option>
          </el-select>

        </el-form-item>
        <el-form-item>
          <el-button type="primary" @click="tagaddd">确定</el-button>
          <el-button @click="givetag = false">取消</el-button>
        </el-form-item>
      </el-form>
    </el-dialog>

    <el-dialog title="添加输入输出文件" :visible="giveinandout" size="tiny" :modal-append-to-body='false' @close='closeDialog'>
      <el-form ref="ainandout" :model="ainandout" label-width="80px">
<!--        <el-form-item label="测试数量" prop="number">-->
<!--          <el-input  v-model="ainandout.number" max-length="10"></el-input>-->
<!--        </el-form-item>-->
<!--        <input type="file" id="file-input" name="fileContent">-->
        <el-dialog :title="title" :visible.sync="open" width="700px" append-to-body>
          <el-form ref="form" :model="form" :rules="rules" label-width="100px">
            <el-form-item label="上传文件" prop="uploadFile">
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
                :http-request="httpRequest"
              :before-upload="beforeUploadFile"
              action="/dev-api/data/file/fileUpload"
              v-hasPermi="['data:file:fileUpload']"
              :file-list="fileList"
              multiple="true"
              show-file-list="true">
              <el-button size="small" type="primary">上传附件</el-button>
              </el-upload>
            </el-form-item>
          </el-form>
          <div slot="footer" class="dialog-footer">
            <el-button type="primary" @click="submitForm(form)">确 定</el-button>
            <el-button @click="cancel">取 消</el-button>
          </div>
        </el-dialog>

        <el-form-item>
          <el-button type="primary" @click="contestedit">确定</el-button>
          <el-button @click="giveinandout = false">取消</el-button>
        </el-form-item>
      </el-form>
    </el-dialog>

    <el-dialog title="添加输入输出样例" :visible="givesimple" size="tiny" :modal-append-to-body='false' @close='closeDialog'>
      <el-form ref="asimple" :model="asimple" label-width="80px">
        <el-form-item label="input">
        <el-col :span="7">
          <el-input v-model="asimple.sampleinput1" placeholder="sample1"></el-input>
        </el-col>
        <el-col class="line" :span="1">-</el-col>
        <el-col :span="7">
          <el-input v-model="asimple.sampleinput2" placeholder="sample2"></el-input>
        </el-col>
        </el-form-item>
        <el-form-item label="output">
        <el-col :span="7">
          <el-input v-model="asimple.sampleoutput1" placeholder="sample1"></el-input>
        </el-col>
        <el-col class="line" :span="1">-</el-col>
        <el-col :span="7">
          <el-input v-model="asimple.sampleoutput2" placeholder="sample2"></el-input>
        </el-col>
        </el-form-item>
        <el-form-item>
          <el-button type="primary" @click="samplee">确定</el-button>
          <el-button @click="givesimple = false">取消</el-button>
        </el-form-item>
      </el-form>
    </el-dialog>

  </div>
</template>

<script>
export function addFile(FormData) {
  return request({
    url: '/data/file/fileUpload',//路径自己调整
    method: 'post',
    data: FormData,
  })
}

export default {
  name: "createproblem",
  data() {
    return {
      givetag:false,
      giveinandout:false,
      givesimple:false,
      form2: {
        problemid: '',
        problemname: '',
        input: '',
        output: '',
        tag:'',
        sampleinput1:'',
        sampleinput2:'',
        sampleoutput1:'',
        sampleoutput2:'',
        description:'',
        hints:''
      },
      atag:{
        tagg:'',
      },
      ainandout:{

      },
      asimple:{
        sampleinput1:'',
        sampleinput2:'',
        sampleoutput1:'',
        sampleoutput2:'',
      },
    }

  },
  methods: {

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
      this.open = false;
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

    submitForm(form) {
      this.$refs.form.validate( valid => {
        if (valid) {
          var upData = new FormData() // 首先创建FormData对象
          this.$refs.upload.submit() // 这里是执行文件上传的函数，其实也就是获取我们要上传的文件
          this.fileList.forEach(function (file) {
            upData.append('uploadFile', file, file.name); // 因为要上传多个文件，所以需要遍历一下才行
            // upData.append('file', this.file); 不要直接使用我们的文件数组进行上传，你会发现传给后台的是两个Object
          })
          //遍历表单中的其他参数
          for(let key in this.form){
            upData.append(key,this.form[key])
          }
          //同理: 将每个参数都进行一次此操作 upData.appen(表单参数,form.表单参数)

          //接口调用
          addFile(upData).then(response => {
            if (response.code === 200) {
              this.msgSuccess("上传成功！");
              this.$refs.upload.clearFiles();		//清空，同this.fileList=[]
              this.open = false;
              this.getList();
            }else{
              this.msgError("上传失败！");
              this.open = false;
              this.fileList = [] // 阻止上传的文件被重复添加，所以清空掉
            }
          })
        }
      })
    },

    // 自定义的上传函数
    httpRequest(param) {
      // 一般情况下是在这里创建FormData对象，但我们需要上传多个文件，为避免发送多次请求，因此在这里只进行文件的获取，param可以拿到文件上传的所有信息
      this.fileList.push(param.file)
    },




    addtag() {
      this.givetag = true;
    },
    addinandout() {
      this.giveinandout = true;
    },
    addsimple() {
      this.givesimple = true;
    },
    closeDialog(){
      this.givetag = false;
      this.givesimple=false;
      this.giveinandout=false;
    },
    tagaddd(){
      form2.tag=atag.tagg;
      this.givetag = false;
    },
    samplee(){
      form2.sampleoutput1=asimple.sampleoutput1;
      form2.sampleoutput2=asimple.sampleoutput2;
      form2.sampleinput1=asimple.sampleinput1;
      form2.sampleinput2=asimple.sampleinput2;
      this.givesimple=false;
    },

    onSubmit(){
      let addproblem=this.form2;
      let {problemid,problemname,input,output,sampleinput1,sampleinput2,sampleoutput1,sampleoutput2,description,hints} = addproblem;
      if(problemid==''||problemname==''||input==''||output==''){
        this.$message.error('新增内容缺失信息')
      }else{
        this.$axios.post("/problemadd", this.form2).then(response => {
          this.$message({
            message: "提交成功！",
            type: "success"
          });
          this.submitid = response.data.id;
        })
          .catch(error => {
            this.$message.error("服务器错误!");
          });
      }
    },


  },

}
</script>

<style scoped>
.el-textarea {
  width: 400px;
}
</style>
