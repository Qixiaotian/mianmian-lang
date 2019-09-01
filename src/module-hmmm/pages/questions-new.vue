<template>
  <div class="dashboard-container">
    <div class="app-container">
      <el-card>
        <el-form class="elform" :model="formData" :rules="rules" ref="formd">
          <!-- 目录下拉列表 -->
          <el-form-item label="学科" prop="subjectID">
            <el-select class="select" v-model="formData.subjectID">
              <el-option
                v-for="item in subjectsList"
                :key="item.value"
                :label="item.label"
                :value="item.value"
              ></el-option>
            </el-select>
          </el-form-item>
          <el-form-item label="目录" prop="catalogID">
            <el-select class="select" v-model="formData.catalogID">
              <el-option
                v-for="item in directorysList"
                :key="item.value"
                :label="item.label"
                :value="item.value"
              ></el-option>
            </el-select>
          </el-form-item>
          <!-- 企业下拉列表 -->
          <el-form-item label="企业" prop="enterpriseID">
            <el-select class="select" v-model="formData.enterpriseID">
              <el-option
                v-for="item in companysList"
                :key="item.id"
                :label="item.company"
                :value="item.id"
              ></el-option>
            </el-select>
          </el-form-item>
          <!-- 方向下拉列表 -->
          <el-form-item label="方向" prop="direction">
            <el-select class="select" v-model="formData.direction">
              <el-option v-for="item in directionList" :key="item" :label="item" :value="item"></el-option>
            </el-select>
          </el-form-item>
          <!-- 城市联动下拉列表 -->
          <el-form-item label="城市" prop="province">
            <el-select
              placeholder="选择城市"
              v-model="formData.province"
              @change="getCity(formData.province)"
            >
              <el-option v-for="item in provincesList" :key="item" :label="item" :value="item"></el-option>
            </el-select>
            <el-select placeholder="选择区域" v-model="formData.city">
              <el-option v-for="item in citysList" :key="item" :label="item" :value="item"></el-option>
            </el-select>
          </el-form-item>
          <!-- 题型单选 -->
          <el-form-item label="题型">
            <el-radio-group v-model="formData.questionType">
              <el-radio :label="1">单选</el-radio>
              <el-radio :label="2">多选</el-radio>
              <el-radio :label="3">简答</el-radio>
            </el-radio-group>
          </el-form-item>
          <!-- 难度单选 -->
          <el-form-item label="难度">
            <el-radio-group v-model="formData.difficulty">
              <el-radio :label="1">简单</el-radio>
              <el-radio :label="2">一般</el-radio>
              <el-radio :label="3">困难</el-radio>
            </el-radio-group>
          </el-form-item>
          <!-- 题干富文本编辑器 -->
          <el-form-item label="题干" style="margin-bottom:50px" prop="question">
            <quill-editor class="edit" v-model="formData.question"></quill-editor>
          </el-form-item>
          <!-- 选项区域-->
          <el-form-item label="选项">
            <el-radio-group v-model="formData.options">
              <el-row type="flex" class="astyle">
                <el-radio :label="1">A</el-radio>
                <el-input placeholder="请输入内容"></el-input>
                <el-upload action :show-file-list="false">
                  <el-button class="photo">上传图片</el-button>
                </el-upload>
              </el-row>
              <el-row type="flex" class="astyle">
                <el-radio :label="2">B</el-radio>
                <el-input placeholder="请输入内容"></el-input>
                <el-upload action :show-file-list="false">
                  <el-button class="photo">上传图片</el-button>
                </el-upload>
              </el-row>
              <el-row type="flex" class="astyle">
                <el-radio :label="3">C</el-radio>
                <el-input placeholder="请输入内容"></el-input>
                <el-upload action :show-file-list="false">
                  <el-button class="photo">上传图片</el-button>
                </el-upload>
              </el-row>
            </el-radio-group>
          </el-form-item>
          <!-- 增加选项和答案 -->
          <el-form-item style="margin:45px 0">
            <el-button type="danger">
              <i class="el-icon-loading"></i>
              增加选项和答案
            </el-button>
          </el-form-item>
          <!-- 上传视频 -->
          <el-form-item
            label="[ 上传视频 ]  :"
            style="padding-bottom:30px;border-bottom:1px solid #ccc"
          >
            <el-button type="primary" style="margin-left:20px">
              <i class="el-icon-upload"></i>
              上传视频
            </el-button>
            <span style="padding-left:20px">未选择任何文件</span>
          </el-form-item>
          <!-- 答案解析 -->
          <el-form-item label="答案解析" style="margin-bottom:50px" prop="answer">
            <quill-editor class="edit" v-model="formData.answer"></quill-editor>
          </el-form-item>
          <!-- 题目备注 -->
          <el-form-item label="题目备注" prop="remarks">
            <el-input
              type="textarea"
              :rows="6"
              style="width:1060px;margin:50px 0 30px -70px"
              placeholder="请输入内容"
              v-model="formData.remarks"
            ></el-input>
          </el-form-item>
          <!-- 试题标签 -->
          <el-form-item label="试题标签" prop="tags">
            <el-select placeholder="请选择" style="width:320px" v-model="formData.tags">
              <el-option
                v-for="item in tagsList"
                :key="item.value"
                :label="item.label"
                :value="item.value"
              ></el-option>
            </el-select>
          </el-form-item>
          <!-- 按钮提交和取消 -->
          <el-row type="flex" justify="center">
            <el-button type="primary" class="jiao" @click="getData">提交</el-button>
            <el-button class="jiao" @click="delData(formData)">取消</el-button>
          </el-row>
        </el-form>
      </el-card>
    </div>
  </div>
</template>

<script>
// 引入学科列表
import { simple as subsimple } from '@/api/hmmm/subjects'
// 引入目录列表
import { simple } from '@/api/hmmm/directorys'
// 引入企业列表
import { list as companysfun } from '@/api/hmmm/companys'
// 引入方向常量
import { direction } from '@/api/hmmm/constants'
// 引入城市
import { provinces, citys } from '@/api/hmmm/citys'
// 引入标签列表
import { simple as tagslis } from '@/api/hmmm/tags'
// 引入添加的方法
import { add } from '@/api/hmmm/questions'

export default {
  async created() {
    // 学科列表
    var jectsimple = await subsimple()
    this.subjectsList = jectsimple.data
    // 目录列表
    var simpleLi = await simple()
    this.directorysList = simpleLi.data
    // 企业列表
    var companys = await companysfun()
    this.companysList = companys.data.items
    // 方向引用
    this.directionList = direction
    //  城市
    var provinceslis = await provinces()
    this.provincesList = provinceslis
    //  标签列表
    var taglis = await tagslis()
    this.tagsList = taglis.data
  },
  name: 'QuestionsNew',
  data() {
    return {
      // 学科列表
      subjectsList: [],
      // 目录列表
      directorysList: [],
      // 企业列表
      companysList: [],
      // 方向列表
      directionList: '',
      // 城市
      provincesList: [],
      // 区域
      citysList: [],
      // 标签列表
      tagsList: [],
      formData: {
        // 学科
        catalogID: '',
        // 目录
        subjectID: '',
        // 企业
        enterpriseID: '',
        // 方向
        direction: '',
        // 城市
        province: '',
        // 区域
        city: '',
        // 题型
        questionType: '1',
        // 难度
        difficulty: '2',
        // 题干
        question: '',
        // 答案解析
        answer: '',
        // 题目备注
        remarks: '',
        // 试题标签
        tags: '',
        // 选项
        options: []
      },
      // 对其属性进行校验
      rules: {
        catalogID: [{ required: true, message: '内容不能为空' }],
        subjectID: [{ required: true, message: '内容不能为空' }],
        enterpriseID: [{ required: true, message: '内容不能为空' }],
        direction: [{ required: true, message: '内容不能为空' }],
        province: [{ required: true, message: '内容不能为空' }],
        question: [{ required: true, message: '内容不能为空' }],
        answer: [{ required: true, message: '内容不能为空' }],
        remarks: [{ required: true, message: '内容不能为空' }],
        tags: [{ required: true, message: '内容不能为空' }]
      }
    }
  },
  methods: {
    // 监听事件传值与区域
    async getCity(pname) {
      var cityslis = await citys(pname)
      this.citysList = cityslis
    },
    delData(params) {
       this.$refs.formd.resetFields()
       this.formData.province = ''
       this.formData.city = ''

    },
    getData() {
      this.$refs.formd
        .validate(async isOk => {
          this.formData.questionType = this.formData.questionType.toString()
          this.formData.difficulty = this.formData.difficulty.toString()
          this.formData.tags = this.formData.tags.toString()
          this.formData.videoURL = 'dsfsf'
          this.formData.options = [
            {code: ''},
             {title: ''},
            { img: '' },
              {isRight: ''}
          ]
          isOk && await add(this.formData)
        this.$router.push('/questions/list')
        })
    }
  }
}
</script>

<style scoped lang="less">
  .elform {
    margin: 30px;
    .select {
      width: 320px;
      height: 38px;
    }
    .astyle {
      width: 600px;
      height: 60px;
      display: flex;
      align-items: center;
      .photo {
        margin-left: 10px;
        width: 103px;
        height: 58px;
        line-height: 20px;
      }
    }
    .edit {
      margin: 50px 0;
      width: 1060px;
      height: 225px;
    }
    .jiao {
      width: 118px;
      height: 38px;
      margin-left: 15px;
    }
  }
</style>
