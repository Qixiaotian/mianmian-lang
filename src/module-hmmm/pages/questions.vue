<template>
  <div class="dashboard-container">
    <el-button class="newAddBtn" icon="el-icon-plus">新增试题</el-button>
    <el-card class="question-tab">
      <el-form v-model="formData" :model="formData" ref="myForm">
        <el-row>
          <el-col :span="6">
            <el-form-item label="学科" label-width="68px" prop="subjectId">
              <el-select style="width:280px" v-model="formData.subjectId">
                <el-option
                  v-for="(item,i) in subjectList"
                  :key="i"
                  :label="item.label"
                  :value="item.value"
                ></el-option>
              </el-select>
            </el-form-item>
          </el-col>
          <el-col :span="6">
            <el-form-item label="难度" label-width="68px" prop="difficulty">
              <el-select
                style="width:280px;height:36px"
                placeholder="请选择"
                v-model="formData.difficulty"
              >
                <el-option
                  v-for="(item,i) in difficultyList"
                  :key="i"
                  :value="item.value"
                  :label="item.label"
                ></el-option>
              </el-select>
            </el-form-item>
          </el-col>
          <el-col :span="6">
            <el-form-item label="试题类型" label-width="68px" prop="questionType">
              <el-select
                style="width:280px;height:36px"
                placeholder="请选择"
                v-model="formData.questionType"
              >
                <el-option
                  v-for="(item,i) in questionTypeList"
                  :key="i"
                  :value="item.value"
                  :label="item.label"
                ></el-option>
              </el-select>
            </el-form-item>
          </el-col>
          <el-col :span="6">
            <el-form-item label="城市" label-width="68px" prop="city">
              <el-select
                style="width:140px"
                v-model="formData.province"
                @change="cityChange(formData.province)"
              >
                <el-option v-for="(item,i) in provincesList" :key="i" :label="item" :value="item"></el-option>
              </el-select>

              <el-select style="width:136px" v-model="formData.city">
                <el-option v-for="(item,i) in cityList" :key="i" :label="item" :value="item"></el-option>
              </el-select>
            </el-form-item>
          </el-col>
        </el-row>
        <el-row>
          <el-col :span="6">
            <el-form-item label="方向" label-width="68px" prop="direction">
              <el-select
                style="width:280px;height:36px"
                placeholder="请输入题目编号/题干"
                v-model="formData.direction"
              >
                <el-option v-for="(item,i) in directionList" :key="i" :label="item" :value="i"></el-option>
              </el-select>
            </el-form-item>
          </el-col>
          <el-col :span="6">
            <el-form-item label="关键字" label-width="68px" prop="keyword">
              <el-input
                v-model="formData.keyword"
                placeholder="请选择"
                style="width:280px;height:36px"
              ></el-input>
            </el-form-item>
          </el-col>
          <el-col :span="6">
            <el-form-item label="题目备注" label-width="68px" prop="remarks">
              <el-input
                v-model="formData.remarks"
                placeholder="请输入"
                style="width:280px;height:36px"
              ></el-input>
            </el-form-item>
          </el-col>
          <el-col :span="6">
            <el-form-item label="二级目录" label-width="68px" prop="catalogID">
              <el-select
                style="width:280px;height:36px"
                placeholder="请选择二级目录"
                v-model="formData.catalogID"
              >
                <el-option
                  v-for="(item,i) in catalogueList"
                  :key="i"
                  :value="item.value"
                  :label="item.label"
                ></el-option>
              </el-select>
            </el-form-item>
          </el-col>
        </el-row>
        <el-row>
          <el-col :span="6">
            <el-form-item label="录入人" label-width="68px" prop="creatorID">
              <el-input
                v-model="formData.creatorID"
                style="width:280px;height:36px"
                placeholder="请输入"
              ></el-input>
            </el-form-item>
          </el-col>
          <el-col :span="6">
            <el-form-item label="标签" label-width="68px" prop="tags">
              <el-select style="width:280px" v-model="formData.tags">
                <el-option
                  v-for="(item,i) in tagsList"
                  :key="i"
                  :label="item.label"
                  :value="item.value"
                ></el-option>
              </el-select>
            </el-form-item>
          </el-col>
          <el-col :span="6">
            <el-form-item label="企业简称" label-width="68px" prop="shortName">
              <el-input style="width:280px" v-model="formData.shortName"></el-input>
            </el-form-item>
          </el-col>
          <el-col :span="6">
            <el-form-item label-width="54px">
              <el-button class="delBtn" @click="resetForm('formData')">清除</el-button>
              <el-button class="searchBtn" @click="searchBtn(formData.keyword)">搜索</el-button>
            </el-form-item>
          </el-col>
        </el-row>
      </el-form>
    </el-card>

    <el-card class="table">
      <el-table :data="tableData" style="width: 100%">
        <el-table-column prop="id" label="序号" width="120" align="center"></el-table-column>
        <el-table-column prop="number" label="试题编号" width="220" align="center"></el-table-column>
        <el-table-column prop="subject" label="学科" align="center"></el-table-column>
        <el-table-column
          :formatter="questionModel"
          prop="questionType"
          label="题型"
          align="center"
          width="120"
        ></el-table-column>
        <el-table-column prop="question" label="题干" align="center"></el-table-column>
        <el-table-column label="录入时间" width="240" align="center">
          <template slot-scope="scope">{{ scope.row.addDate | parseTimeByString}}</template>
        </el-table-column>
        <el-table-column :formatter="difficulty" prop="difficulty" label="难度" align="center"></el-table-column>
        <el-table-column prop="creator" label="录入人" align="center"></el-table-column>
        <el-table-column prop="address" label="使用次数" align="center"></el-table-column>
        <el-table-column label="操作" align="center">
          <template slot-scope="obj">
            <el-dropdown trigger="click" @command="calssified">
              <el-link type="primary">预览</el-link>
              <el-dropdown-menu slot="dropdown">
                <el-dropdown-item command="single">单选</el-dropdown-item>
                <el-dropdown-item command="multiple">多选</el-dropdown-item>
                <el-dropdown-item command="short">简答</el-dropdown-item>
              </el-dropdown-menu>
            </el-dropdown>
            <el-link type="primary">禁用</el-link>
            <el-link type="primary" @click="delQuestion(obj.row)">删除</el-link>
            <el-link type="primary">下架</el-link>
          </template>
        </el-table-column>
      </el-table>
      <el-dialog :visible.sync="dialogVisible" width="50%">
        <classify-box :dialogVisible="dialogVisible"></classify-box>
        <span slot="footer" class="dialog-footer" style="margin-top:-200px">
          <el-button @click="dialogVisible = false">关闭</el-button>
        </span>
      </el-dialog>

      <el-row style="margin:40px 0">
        <el-pagination
          :page-size="page.pagesize"
          layout="prev, pager, next, jumper"
          :total="page.pages"
        ></el-pagination>
      </el-row>
    </el-card>
  </div>
</template>

<script>
// 难度
import { difficulty, direction, questionType } from '@/api/hmmm/constants'
import { simple } from '@/api/hmmm/tags'
import { simple as subjects } from '@/api/hmmm/subjects'
import { simple as tagSimple } from '@/api/hmmm/directorys'
import { list, remove } from '@/api/hmmm/questions'
import { provinces, citys } from '@/api/hmmm/citys' // 引入城市模块
export default {
  name: 'QuestionsList',
  data() {
    return {
      dialogVisible: false, // 组件传值
      difficultyList: difficulty,
      directionList: direction,
      questionTypeList: questionType,
      catalogueList: [],
      subjectList: [],
      tagsList: [],
      provincesList: [], // 城市省会数组
      cityList: [], // 城市数组
      formData: {
        subjectId: '', // 学科
        direction,
        difficulty, // 难度
        tags: '',
        questionType, // 试题类型
        catalogID: '',
        company: '',
        creatorID: '', // 录入人
        keyword: '',
        province: '', // 城市省会
        city: '',
        remarks: '',
        shortName: ''
      },
      tableData: [],
      page: {
        page: 0,
        pagesize: 1,
        pages: 1
      }
    }
  },
  created() {
    this.getTag()
    this.getDirectory()
    this.getTables()
    this.getSubject() // 学科
    this.getProvinces() // 获取省会
  },
  methods: {
    // 点击预览触发的事件 例如:单选,多选,简单
    calssified(command) {
      if (command === 'single') {
        this.dialogVisible = true
      }
    },
    // 难度过滤
    difficulty(row, column, cellValue, index) {
      var obj = difficulty.find(
        item => Number(item.value) === Number(cellValue)
      )
      return obj.label
    },
    // 题型过滤
    questionModel(row, column, cellValue, index) {
      var obj = questionType.find(item => {
        return Number(item.value) === Number(cellValue)
      })
      return obj.label
    },
    // 删除
    async delQuestion(obj) {
      await this.$confirm('您确定要删除吗')
      await remove(obj)
      this.getTables()
    },
    // 搜索
    searchBtn(value) {
      if (value === '') {
        this.getTables()
      }
      this.tableData = this.tableData.filter(
        item => item.question.indexOf(value) > -1
      )
    },

    // 获取标签
    async getTag() {
      let res = await simple()
      this.tagsList = res.data
    },

    // 目录
    async getDirectory() {
      let res = await tagSimple()
      this.catalogueList = res.data
    },

    // 获取列表
    async getTables() {
      let datas = this.formData
      let res = await list(datas)
      this.tableData = res.data.items
      this.page.pages = res.data.pages
    },

    // 学科
    async getSubject() {
      let res = await subjects()
      this.subjectList = res.data
    },

    // 重置
    resetForm() {
      this.formData.province = ''
      this.formData.city = ''
      this.$refs.myForm.resetFields()
    },

    // 城市省会
    async getProvinces() {
      let res = await provinces()
      this.provincesList = res
    },

    // 城市省会变化时触发
    cityChange(pname) {
      var res = citys(pname)
      this.cityList = res
    }
  }
}
</script>

<style scoped>
.newAddBtn {
  width: 125px;
  height: 40px;
  color: white;
  font-size: 16px;
  margin-left: 16px;
  margin-top: 23px;

  background: -webkit-linear-gradient(
    left,
    #1493fa,
    #02c4fa
  ); /* Safari 5.1 - 6.0 */
}
.question-tab {
  margin: 16px;
  padding: 10px;
}
.searchBtn {
  background: -webkit-linear-gradient(
    left,
    #1493fa,
    #02c4fa
  ); /* Safari 5.1 - 6.0 */
  color: white;
  width: 94px;
}
.delBtn {
  background: -webkit-linear-gradient(#fefffe, #f5f4f3); /* Safari 5.1 - 6.0 */
  width: 94px;
  margin-left: 14px;
}
.table {
  margin: 16px;
  padding: 10px;
  text-align: center;
}
.btnList {
  display: flex;
  flex-direction: column;
  text-align: center;
}
</style>
