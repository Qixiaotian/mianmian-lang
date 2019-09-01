<template>
  <div class="dashboard-container">
    <el-row>
      <el-button class="button-zhu" type="primary">主要按钮</el-button>
    </el-row>
    <!-- 卡片1 -->
    <el-card class="box-card box-card1">
      <el-form ref="formData" :model="formData" label-width="140px">
        <!-- 第一行 -->
        <el-row :gutter="20">
          <el-col :span="6">
            <el-form-item label="学科">
              <el-select @change="screen" v-model="formData.subjectID" placeholder="请选择">
                <el-option
                  v-for="item in subjectIDList"
                  :key="item.value"
                  :label="item.label"
                  :value="item.value"
                ></el-option>
              </el-select>
            </el-form-item>
          </el-col>
          <el-col :span="6">
            <el-form-item label="状态">
              <el-select @change=" screen" v-model="formData.publishType" placeholder="请选择">
                <el-option
                  v-for="item in publishTypeList"
                  :key="item.value"
                  :label="item.label"
                  :value="item.value"
                ></el-option>
              </el-select>
            </el-form-item>
          </el-col>
          <el-col :span="6">
            <el-form-item label="难度">
              <el-select @change=" screen" v-model="formData.difficulty" placeholder="请选择">
                <el-option
                  v-for="item in difficultyList"
                  :key="item.value"
                  :label="item.label"
                  :value="item.value"
                ></el-option>
              </el-select>
            </el-form-item>
          </el-col>
          <el-col :span="6">
            <el-form-item label="试题类型">
              <el-select @change=" screen" v-model="formData.questionType" placeholder="请选择">
                <el-option
                  v-for="item in questionTypeList"
                  :key="item.value"
                  :label="item.label"
                  :value="item.value"
                ></el-option>
              </el-select>
            </el-form-item>
          </el-col>
        </el-row>
        <!-- 第二行 -->
        <el-row :gutter="20">
          <el-col :span="6">
            <el-form-item label="标签">
              <el-input v-model="formData.keyword" style="width:220px" placeholder="请选择"></el-input>
            </el-form-item>
          </el-col>
          <el-col :span="6">
            <el-form-item label="录入人">
              <el-input v-model="formData.creatorID" style="width:220px" placeholder="请选择"></el-input>
            </el-form-item>
          </el-col>
          <el-col :span="6">
            <el-form-item label="关键字">
              <el-input
                @change="screen"
                v-model="formData.keyword"
                placeholder="请输入题目编号/题干"
                style="width:220px"
              ></el-input>
            </el-form-item>
          </el-col>
          <el-col :span="6">
            <el-form-item label="题目备注">
              <el-input v-model="formData.remarks" placeholder="请输入" style="width:220px"></el-input>
            </el-form-item>
          </el-col>
        </el-row>
        <!-- 第三行 -->
        <el-row :gutter="20">
          <el-col :span="6">
            <el-form-item label="二级目录">
              <el-input v-model="formData.entering" placeholder="请输入二级目录" style="width:220px"></el-input>
            </el-form-item>
          </el-col>
          <el-col :span="6">
            <el-form-item label="城市">
              <el-select @change="MultiCitySwitch" v-model="formData.province" placeholder="请选择">
                <el-option v-for="item in provinceList" :key="item" :label="item" :value="item"></el-option>
              </el-select>

              <el-select
                @change=" screen"
                style="margin-top:5px"
                v-model="formData.city"
                placeholder="请选择"
              >
                <el-option v-for="item in citysList" :key="item" :label="item" :value="item"></el-option>
              </el-select>
            </el-form-item>
          </el-col>
          <el-col :span="6">
            <el-form-item label="企业简称">
              <el-input v-model="formData.shortName" placeholder="请输入二级目录" style="width:220px"></el-input>
            </el-form-item>
          </el-col>
          <el-col :span="6">
            <el-form-item label="方向">
              <el-select @change=" screen" v-model="formData.direction" placeholder="请选择">
                <el-option v-for="item in directionList" :key="item" :label="item" :value="item"></el-option>
              </el-select>
            </el-form-item>
          </el-col>
        </el-row>
      </el-form>
    </el-card>
    <!-- 第二个卡片 -->
    <el-card class="box-card box-card2">
      <!-- 表格 -->
      <el-table :data="tableData" style="width: 100%">
        <el-table-column align="center" type="index" label="序号" width="80"></el-table-column>
        <el-table-column align="center" prop="number" label="试题编号" width="93"></el-table-column>
        <el-table-column
          align="center"
          :formatter="subjectIDfun"
          prop="subjectID"
          label="学科"
          width="80"
        ></el-table-column>
        <el-table-column
          align="center"
          :formatter="questionTypefun"
          prop="questionType"
          label="题型"
          width="123"
        ></el-table-column>
        <el-table-column align="center" prop="question" label="题干" width="163"></el-table-column>
        <el-table-column align="center" prop="addDate" label="录入时间" width="145"></el-table-column>
        <el-table-column align="center" prop="creator" label="录入人" width="113"></el-table-column>
        <el-table-column
          align="center"
          :formatter="difficultyfun"
          prop="difficulty"
          label="难度"
          width="63"
        ></el-table-column>
        <el-table-column align="center" prop="name" label="使用次数" width="113"></el-table-column>
        <el-table-column
          align="center"
          :formatter="chkStatefun"
          prop="chkState"
          label="审核状态"
          width="113"
        ></el-table-column>
        <el-table-column align="center" prop="chkRemarks" label="盛审核意见" width="103"></el-table-column>
        <el-table-column align="center" prop="chkUser" label="审核人" width="103"></el-table-column>
        <el-table-column
          align="center"
          :formatter="formatter"
          prop="publishState"
          label="发布状态"
          width="113"
        ></el-table-column>
        <el-table-column align="center" prop="name" label="操作" width="230">
          <template slot-scope="obj">
            <el-popover placement="right" trigger="click">
              <el-row>
                <el-button size="mini">单选</el-button>
                <el-button size="mini">多选</el-button>
                <el-button size="mini">简答</el-button>
              </el-row>
              <el-button slot="reference" type="text">审核</el-button>
            </el-popover>
            <el-button type="text">预览</el-button>            
            <el-button type="text">下架</el-button>
            <el-button type="text">修改</el-button>
            <el-button type="text" @click="cancel(obj)">删除</el-button>
          </template>
        </el-table-column>
      </el-table>
      <el-col>
        <el-pagination
          class="page"
          type="flex"
          align="center"
          background
          layout="prev, pager, next"
          :total="page.total"
          :page-size="page.pagesize"
          @current-change="changPage"
        ></el-pagination>
      </el-col>
    </el-card>
  </div>
</template>

<script>
//  难度 方向 试题类型  状态
import {
  difficulty,
  direction,
  questionType,
  publishType
} from '@/api/hmmm/constants'
// 学科
import { simple } from '@/api/hmmm/subjects'
// 城市
import { provinces, citys } from '@/api/hmmm/citys'
// 表格
import { choice, remove } from '@/api/hmmm/questions'

export default {
  name: 'QuestionsChoice',
  data() {
    return {
      // 状态
      publishTypeList: [],
      // 难度
      difficultyList: [],
      // 方向
      directionList: [],
      // 试题类型
      questionTypeList: [],
      // 学科
      subjectIDList: [],
      // 城市
      provinceList: [],
      citysList: [],
      // 分页的对象
      page: {
        // 总条数
        total: 1,
        // 当前所需页数
        pagesize: 4,
        // 当前页数
        currentPage: 1
      },
      // 表格
      tableData: [],
      // 搜索表单所有数据总结对象
      formData: {
        // 学科
        subjectID: '',
        // 状态
        chkState: '',
        // 难度
        difficulty: '',
        // 试题类型
        questionType: '',
        // 标签
        tags: '',
        // 录入人
        creatorID: '',
        // 关键字
        keyword: '',
        // 题目备注
        remarks: '',
        // 二级目录
        entering: '',
        // 城市
        province: '',
        city: '',
        // 企业简称
        shortName: '',
        // 方向
        direction: ''
      }
    }
  },
  // =========================================================================================================================================
  methods: {
    // 封装一个获取内容函数
    async getTableList() {
      let n1 = await choice({
        page: this.page.currentPage,
        pagesize: this.page.pagesize,
        ...this.formData
      })
      this.tableData = n1.data.items
    },
    // 搜索栏绑定的事件
    async screen() {
      this.page.currentPage = 1
      this.getTableList()
    },
    // 分页当前页数事件
    async changPage(newpage) {
      this.page.currentPage = newpage
      this.getTableList()
    },
    // 城市事件绑定区选择器
    async MultiCitySwitch() {
      this.citysList = await citys(this.formData.province)
    },
    // 删除按钮
    cancel(obj) {
      this.$confirm('您确定要删除吗？', '提示').then(async () => {
        await remove(obj.row)
        this.getTableList()
      })
    },
    // 审核状态
    chkStatefun(row, column, cellValue, index) {
      if (cellValue === 0) {
        return '待审核'
      } else if (cellValue === 1) {
        return '通过'
      } else {
        return '拒绝'
      }
    },
    // 题型
    questionTypefun(row, column, cellValue, index) {
      if (cellValue === '1') {
        return '单选'
      } else if (cellValue === '2') {
        return '多选'
      } else {
        return '简答'
      }
    },
    // 学科
    subjectIDfun(row, column, cellValue, index) {
      if (cellValue === 1) {
        return 'java'
      } else if (cellValue === 2) {
        return 'ios'
      } else if (cellValue === 3) {
        return '安卓'
      } else if (cellValue === 4) {
        return '前端'
      } else if (cellValue === 5) {
        return '设计'
      } else if (cellValue === 6) {
        return '产品'
      } else if (cellValue === 7) {
        return 'c++'
      } else if (cellValue === 8) {
        return '数据库'
      } else if (cellValue === 9) {
        return '算法'
      } else if (cellValue === 10) {
        return '运维'
      } else if (cellValue === 11) {
        return 'php'
      } else if (cellValue === 12) {
        return 'C#'
      } else if (cellValue === 13) {
        return 'C'
      } else if (cellValue === 14) {
        return 'python'
      } else if (cellValue === 15) {
        return '大数据'
      }
    },
    // 难度
    difficultyfun(row, column, cellValue, index) {
      if (cellValue === '0') {
        return '简单'
      } else if (cellValue === '1') {
        return '一般'
      } else {
        return '困难'
      }
    },
    // 发布状态
    formatter(row, column, cellValue, index) {
      if (cellValue === 0) {
        return '待发布'
      } else if (cellValue === 1) {
        return '已发布'
      } else {
        return '已下架'
      }
    }
  },
  async created() {
    // 状态
    this.publishTypeList = publishType
    // 难度
    this.difficultyList = difficulty
    // 方向
    this.directionList = direction
    // 试题类型
    this.questionTypeList = questionType
    // 学科
    let n1 = await simple()
    this.subjectIDList = n1.data
    // 城市
    this.provinceList = await provinces()
    // 表格
    let n2 = await choice()
    // this.tableData = n2.data.items
    this.getTableList()
    this.page.total = n2.data.counts
    // console.log(n2)
  }
}
</script>

<style scoped>
.button-zhu {
  position: absolute;
  top: -10px;
}
.dashboard-container {
  padding: 20px;
  position: relative;
}
.box-card1 {
  width: 100%;
  height: 230px;
  margin-top: 40px;
}
.box-card2 {
  width: 100%;
  height: 500px;
  margin-top: 20px;
}
.page {
  margin-top: 20px;
}
</style>
