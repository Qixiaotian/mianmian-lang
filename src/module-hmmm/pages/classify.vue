<template>
  <el-dialog :visible.sync="dialogVisible" width="50%">
    <div slot="title">题目预览</div>
    <el-form v-model="formData">
      <el-form-item>
        <el-row>
          <el-col :span="12">
            <span>【题库】:</span>
            <span>面试宝典题库</span>
          </el-col>
          <el-col :span="12">
            <span>【学科】:</span>
            <span>JavaEE</span>
          </el-col>
          <el-col :span="6">
            <span>【题型】:</span>
            <span>单选题</span>
          </el-col>
          <el-col :span="6">
            <span>【编号】:</span>
            <span>MS000002</span>
          </el-col>
          <el-col :span="6">
            <span>【难度】:</span>
            <span>一般</span>
          </el-col>
          <el-col :span="6">
            <span>【标签】:</span>
            <span>JavaScript</span>
          </el-col>
          <el-col :span="6">
            <span>【目录】:</span>
            <span>JavaScript</span>
          </el-col>
          <el-col :span="6">
            <span>【企业】:</span>
            <span>阿里巴巴</span>
          </el-col>
          <el-col :span="6">
            <span>【方向】:</span>
            <span>金融</span>
          </el-col>
          <el-col :span="6">
            <span>【城市】:</span>
            <span>北京</span>
          </el-col>
        </el-row>
        <div class="line"></div>
      </el-form-item>
      <el-form-item>
        <div class="title">【题干】:</div>
        <p
          style="width:505px;line-height:24px;margin:0;"
        >对效果图进行高仿，通过本考题重点了解学生对代码进阶阶段所学知识的综合应用单选题选项:（以下选中的选项为正确答案）</p>
        <el-radio-group v-model="formData.radio">
          <el-radio :label="1">A:选项一</el-radio>
          <el-radio :label="2">B:选项二</el-radio>
          <el-radio :label="3">C:选项三</el-radio>
          <el-radio :label="4">D:选项四</el-radio>
        </el-radio-group>
        <div class="line"></div>
      </el-form-item>
      <el-form-item style="margin-top:20px">
        <span>【参考答案】:</span>
        <el-button>视频答案预览</el-button>
        <div class="line"></div>
      </el-form-item>
      <el-form-item>
        <span>【答案解析】:</span>
        <p style="margin:0">对效果图进行高仿，通过本题重点了解学生对代码阶段所学知识的综合应用</p>
        <div class="line"></div>
      </el-form-item>
      <el-form-item>
        <span>【题目备注】:</span>
        <p style="margin:0">对效果图进行高仿，通过本题重点了解学生对代码阶段所学知识的综合应用</p>
      </el-form-item>
    </el-form>
    <span slot="footer" class="dialog-footer" style="margin-top:-200px">
      <el-button @click="dialogVisible = false">关闭</el-button>
    </span>
  </el-dialog>
</template>

<script>
export default {
  data() {
    return {
      // dialogVisible: true,
      formData: {
        radio: 1
      }
    }
  },
  props: ['dialogVisible'],
  methods: {
    handleClose(done) {
      this.$confirm('确认关闭？')
        .then(_ => {
          done()
        })
        .catch(_ => {})
    },
    fn() {
      console.log(this.dialogVisible)
    }
  },
  created() {
    this.fn()
  }
}
</script>

<style>
.el-dialog__header {
  background-color: #fff !important;
  text-align: left;
}
.el-dialog__body {
  text-align: left;
  padding: 10px;
}
.el-form-item__content {
  font-size: 16px;
}
.line {
  width: 100%;
  height: 1px;
  background-color: rgb(231, 231, 231);
  margin-top: 20px;
}
.el-dialog__headerbtn .el-dialog__close {
  color: black;
}
.el-dialog__headerbtn .el-dialog__close:hover {
  color: rgb(86, 86, 255);
}
</style>
