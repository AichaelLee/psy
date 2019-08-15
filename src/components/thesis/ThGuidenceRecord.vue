<template>
  <div>

    <el-row :gutter="5">
      <el-col :xs="12" :sm="6" :md="6" :lg="6" :xl="6" :offset="1"  style="text-aligin:left margin-top:5px"><FilenameOption v-model="filename" /></el-col>
      
      <el-col style="margin-top:5px" :xs="24"  :sm="14" :md="11" :lg="8" :xl="8"><AutoWidthOption v-model="autoWidth" /></el-col>
     <el-col style="margin-top:5px"  :xs="12"  :sm="7" :md="6" :lg="4" :xl="4"> <BookTypeOption v-model="bookType" /></el-col>
      <el-col style="margin-top:5px"  :xs="4"  :sm="4" :md="4" :lg="4" :xl="4"><el-button  :loading="downloadLoading" style="margin:0 0 20px 20px;" type="primary" icon="document" @click="handleDownload">导出文件</el-button></el-col>
    </el-row>
    <br>
    <!-- 表格 -->
    <el-row>
      <el-col :sm="4" :md="4" :lg="4" :xl="4"  class="hidden-sm-and-down" >&nbsp;</el-col>
      <el-col :xs="24" :sm="24" :md="24" :lg="24" :xl="24">
         <el-table v-loading="listLoading" :data="tableDataView" element-loading-text="拼命加载中" border fit highlight-current-row 

         :default-sort = "{prop: 'id', order: 'descending',prop:'age',order:'descending',prop:'unba',order:'descending',prop:'sumba',order:'descending'}"
         >
      <el-table-column align="center"  prop="id" label="Id" sortable width="110">
        <template slot-scope="scope">
          {{ scope.row.id}}
        </template>
      </el-table-column>
       
      <el-table-column label="年龄" prop="age" width="110" sortable align="center">
        <template slot-scope="scope">
          {{ scope.row.age }}
        </template>
      </el-table-column>

       <el-table-column label="性别" width="110" sortable align="center">
        <template slot-scope="scope">
          <el-tag>{{ scope.row.gender=='1'?'男':'女'}}</el-tag>
        </template>
      </el-table-column>
      
      <el-table-column prop="r1" label="第一回合" sortable width="150" align="center">
         <template slot-scope="scope">
          {{ scope.row.r1}}
        </template>
      </el-table-column>
      <el-table-column prop="r2" label="第二回合" sortable width="150" align="center">
            <template slot-scope="scope">
          {{ scope.row.r2}}
        </template>
         
      </el-table-column>
      <el-table-column prop="r3" label="第三回合" sortable width="150" align="center">
            <template slot-scope="scope">
        {{ scope.row.r3}}
        </template>
          
      </el-table-column>
      <el-table-column prop="r4" label="第四回合" sortable width="150" align="center">
           
      <template slot-scope="scope">
           {{ scope.row.r4}}
      </template>
      </el-table-column>
      <el-table-column prop="r5" label="第五回合" sortable width="150" align="center">
        <template slot-scope="scope">
         {{ scope.row.r5}}
        </template>
            
      </el-table-column>
      <el-table-column prop="r6" label="第六回合" sortable width="150" align="center">
        <template slot-scope="scope">
         {{ scope.row.r6}}
        </template>
            
      </el-table-column>
      <el-table-column prop="r7" label="第七回合" sortable width="150" align="center">
        <template slot-scope="scope">
          {{ scope.row.r7}}
        </template>
           
      </el-table-column>
      <el-table-column prop="r8" label="第八回合" sortable width="150" align="center">
        <template slot-scope="scope">
           {{ scope.row.r8}}
        </template>
           
      </el-table-column>
      
      <el-table-column prop="r9" label="第九回合" sortable width="150" align="center">
        <template slot-scope="scope">
           {{ scope.row.r9}}
        </template>
           
      </el-table-column>
      <el-table-column prop="r10" label="第十回合" sortable width="150" align="center">
        <template slot-scope="scope">
         {{ scope.row.r10}}
        </template>
            
      </el-table-column>
      <el-table-column prop="r11" label="第十一回合" sortable width="150" align="center">
        <template slot-scope="scope">
          {{ scope.row.r11}}
        </template>
           
      </el-table-column>
      <el-table-column prop="r12" label="第十二回合" sortable width="150" align="center">
        <template slot-scope="scope">
           {{ scope.row.r12}}
        </template>
           
      </el-table-column>


      

        <el-table-column prop="sumba" label="实验报酬" width="110" sortable align="center">
        <template slot-scope="scope">
          {{ scope.row.finalGrade }}
        </template>
      </el-table-column>
      <!-- <el-table-column label="Readings" width="115" align="center">
        <template slot-scope="scope">
          {{ scope.row.pageviews }}
        </template>
      </el-table-column> -->
      <el-table-column align="center" label="日期" sortable width="200">
        <template slot-scope="scope">
          <i class="el-icon-time"/>
          <span>{{ scope.row.timestamp | parseTimestamp }}</span>
        </template>
      </el-table-column>
             <el-table-column label="操作" width="140" align="center">
          <template slot-scope="scope">
            <el-button size="mini" type="danger" plain  @click="handleDelete(scope.$index, scope.row)"><i class="fa fa-trash-o"></i></el-button>
          </template>
          </el-table-column>
    </el-table>
      </el-col>

    </el-row>
          <el-row >
        <el-col :xs="24" :sm="24" :md="24" :lg="24" :xl="24"  style="margin-top:10px">
          <el-pagination background  @size-change="handleSizeChange"
          @current-change="handleCurrentChange"
          :current-page="pagenum"
          :page-sizes="[5,10,15,20,25]"
          :page-size="pageSize"
          :layout="pagelayout"
          :total="tableSize">
        </el-pagination>
        </el-col>
      </el-row>
   
  </div>
</template>

<script>
import { parseTime } from '@/utils/index'
import 'element-ui/lib/theme-chalk/display.css';
// options components
import FilenameOption from './components/FilenameOption'
import AutoWidthOption from './components/AutoWidthOption'
import BookTypeOption from './components/BookTypeOption'

export default {
  name: 'ExportExcel',
  components: { FilenameOption, AutoWidthOption, BookTypeOption },
  computed:{
    bartInfo(){
      
        return this.$store.state.bartData;
      },
            // 页数小于2的时候去掉jumper
      pagelayout: function() {
        let sumpagenum = (this.tableSize / this.pageSize) + 1
        if ((this.tableSize % this.pageSize) === 0) {
          sumpagenum = (this.tableSize / this.pageSize)
        }
        if (sumpagenum < 2) {
          return 'total, sizes, prev, pager, next'
        } else {
          return 'total, sizes, prev, pager, next, jumper'
        }
      },
          // 分页的筛选
      tableDataView: function() {
        return this.list.slice((this.pagenum - 1) * this.pageSize, this.pagenum * this.pageSize)
      }
     
  },
  data() {
    return {
      // list: [{ id: 1, name:'张三',age: '24', gender: '男', sumScore:'80' ,timestamp: '2016-05-04'},
      //   { id: 2,name:'李四',age: '24', gender: '女', sumScore:'90', timestamp: '2016-05-04' }],
      list:[],
      listLoading: true,
      downloadLoading: false,
      filename: '',
      autoWidth: true,
      bookType: 'xlsx',
       // 当前第几页
        pagenum: 1,
        // 每页条数
        pageSize: 5,
        tableSize:5
    }
  },
  created() {
    this.fetchData();
    this.create_websql();
    
    
  },
  filters: {
  parseTimef: function (value) {
    return parseTime(value)
  },
  parseTimestamp(val) {
          if (val != null) {
                  var date = new Date(val);
                  return date.getFullYear() + '-' + (date.getMonth() + 1) + '-' + date.getDate();
              }
      },
  pargeGender(value){
    alert(value)
    return value===1?value='男':value='女';
  }
},
  methods: {
      // 改变每页条数
      handleSizeChange(val) {
        this.pageSize = val
      },
        // 计算开始数和结束的数
      handleCurrentChange(val) {
        this.pagenum = val
      },
  
      create_websql() {
        //
          this.db = openDatabase('psyDB', '1.0', 'psyDB DB', 2 * 1024 * 1024);
          let _this = this;
          _this.list=[];
          this.db.transaction(function (tx) {
              tx.executeSql('SELECT * FROM PSYT', [], function (tx, results) {
                  var len = results.rows.length,i;
                  _this.tableSize = len
                  let msg = "<p>查询记录条数: " + len + "</p>";
                  for (i = 0; i < len; i++){
                    //alert(results.rows.item(i).id );
                    
                    _this.list.push(results.rows.item(i))
                  
                  }
              }, null);
            });
      },
    fetchData() {
      this.listLoading = true
      this.listLoading = false
     
    },
          // table数据的删除
      handleDelete(index, row) {
        this.$confirm('请问要删除此条数据吗', '注意', {
          confirmButtonText: '是',
          cancelButtonText: '否',
          type: 'warning'
        }).then(() => {
             let _this = this
            this.db = openDatabase('psyDB', '1.0', 'psyDB DB', 2 * 1024 * 1024);
            this.db.transaction(function (tx) {
            tx.executeSql('DELETE FROM PSYT WHERE id=?',[row.id]);
            
             });
            this.create_websql();
        }).catch(() => {})
      },
    handleDownload() {
      this.downloadLoading = true
      import('@/vendor/Export2Excel').then(excel => {
        const tHeader = ['Id', '年龄', '性别', 
        '第一回合',
        '第二回合',
        '第三回合',
        '第四回合',
        '第五回合',
        '第六回合',
        '第七回合',
        '第八回合',
        '第九回合',
        '第十回合',
        '第十一回合',
        '第十二回合'
        ,'实验报酬','日期']
        const filterVal = ['id', 'age', 'gender',
        'r1',
        'r2',
        'r3',
        'r4',
        'r5',
        'r6',
        'r7',
        'r8',
        'r9',
        'r10',
        'r11',
        'r12',
        'finalGrade', 'timestamp']
        const list = this.list
        const data = this.formatJson(filterVal, list)
        console.log(data)
        excel.export_json_to_excel({
          header: tHeader,
          data,
          filename: this.filename,
          autoWidth: this.autoWidth,
          bookType: this.bookType
        })
        this.downloadLoading = false
      })
    },
    formatJson(filterVal, jsonData) {
      return jsonData.map(v => filterVal.map(j => {
        if (j === 'timestamp') {
          return parseTime(v[j])
        } else {
          return v[j]
        }
      }))
    }
  }
}
</script>

<style lang="scss">
.radio-label {
  font-size: 14px;
  color: #606266;
  line-height: 40px;
  padding: 0 12px 0 30px;
}

</style>

