<template>
  <!-- <div>
    <cnki-upload ref='uploader' class="example_upload" pickId="#test" pickText="选择文件"></cnki-upload>
    <div class='example_upload' id="test"></div>
    <el-button @click="test">你要做什么操作啊</el-button>
 
  </div> -->
  <div>
    <el-row>
      <el-col :span="24">

           <div>
      <div class="tips-wrap">
        <div class="tips-title">
          <i></i>
          <span>实验提示</span>
        </div>
        <el-row>
          <el-col :span="12">
              现在你拥有 <span style="color:#dd4b39; font-weight:bold; font-size:16px;">{{this.currentCapital}}</span>  分，请选择投入<br>
              对于你选择投入的部分，1/2 的可能性会损失，1/2的可能性会扩大2.5倍
          </el-col>
        </el-row>
       
       </div>

        <div >
          <div class="cnki-wrap">

          <!-- 顶头栏信息 -->
            <el-row>
             <el-form :model="ruleForm" :rules="rules" ref="ruleForm" label-width="0px" class="demo-ruleForm">
                <el-col :offset="1" :xs="8" :sm="8" :md="4" :lg="4" :xl="4" style="margin-top:10px">
                    <el-form-item  prop="id">
                    <el-input
                      placeholder="序号"
                      v-model.number="ruleForm.id"
                      clearable>
                    </el-input>
                    </el-form-item>
                </el-col>

                <el-col :xs="6" :sm="6" :md="2" :lg="2" :xl="2" style="margin-top:10px">
                      <el-form-item  prop="gender">
                        <el-select v-model="ruleForm.gender" clearable placeholder="性别">
                        <el-option
                          v-for="item in options"
                          :key="item.value"
                          :label="item.label"
                          :value="item.value">
                        </el-option>
                      </el-select>
                      </el-form-item>
                </el-col>

               <el-col :xs="6" :sm="6" :md="2" :lg="2" :xl="2" style="margin-top:10px">
                      <el-form-item  prop="age">
                      <el-input
                      placeholder="年龄"
                      v-model.number="ruleForm.age"
                      clearable>
                    </el-input>
                      </el-form-item>
                </el-col>
               
                <el-col :xs="14" :sm="14" :md="3" :lg="3" :xl="3" style="margin-top:10px">
                      <el-button type="primary" plain @click="saveInfo('ruleForm')" >开始</el-button>
                </el-col>

               </el-form>
                      
            </el-row>


             <el-row>
                <el-col :span="12" :offset="1" style="margin-top:18px">
                    已进行回合数:{{roundNum}}
                    <!-- <span v-if="roundNum === 12">
                      当前结果 ：{{revenue}}
                    </span> -->
                     <br> 当前结果 ：{{revenue}}
                </el-col>
              </el-row>
                
              <el-row style="margin-top:18px">
                <el-col :span="12" :offset="1">
                    <el-input-number size="small" :disabled="disabled" v-model="inputStake" placeholder="请输入赌注"></el-input-number>
                </el-col>
              </el-row>

              <el-row>
                <el-col :span="12" :offset="1" style="margin-top:18px">
                    <el-button size="small" round @click="nextRound" type="primary" :disabled = "disabled">下一回合</el-button>
                </el-col>
              </el-row>

          </div>
        </div>

   </div>

      </el-col>
    </el-row>
  </div>
  
</template>

<script>

export default {
  name: 'Login',
  data () {
      return {
         ruleForm:{
        id:'',
        gender:'',
        age:''
      },
        options: [{
          value: '1',
          label: '男'
        }, {
          value: '2',
          label: '女'
        }
        ],
        infoDone:false,
        // 回合总数
        roundNum:0,
        // 投入赌注：
        inputStake:'',
        // 当前可支配的本金
        currentCapital:20,
        // 后三个可支配的本金
        last3Capital:'',

        // 回报
        revenue:0,
        disabled:true,
        rules: {
          id: [
             { required: true, message: '序号不能为空'},
            { type: 'number', message: '序号必须为数字值'}
          ],
          gender: [
            { required: true, message: '请选择性别', trigger: 'change' }
          ],
          age: [
            { required: true, message: '年龄不能为空'},
            { type: 'number', message: '年龄必须为数字值'}
          ]
    }
      }
  },
  mounted(){
    this.create_websql();
  },
   watch: {
    // 当前输入不能大于本金
    inputStake: function (newInput, oldInput) {
      if(newInput<0){
         this.$message.error('本金数必须大于0');
         this.inputStake = ''
      }
      if(newInput>this.currentCapital){
         this.$message.error('当前输入不能大于本金');
         this.inputStake = ''
      }
    },
    roundNum(newO,oO){
      if(newO===12){
              // 已经是最后一个回合了,insert db
            let _this = this
            this.db.transaction(function (tx) {
            tx.executeSql('INSERT INTO PSYT (id, gender, age, finalGrade,timestamp) VALUES (?, ?,?, ?, ?,?)',[_this.ruleForm.id,_this.ruleForm.gender, _this.ruleForm.age, _this.revenue,new Date()]);
            
             });
      }
    }

  },
  methods: {
    test () {
      // 这是有用的数据
      console.log(this.$refs.uploader.fileList)
    },
    // 下一回合
    nextRound(){
   
      if(this.inputStake === ''){
        this.$message.error('请输入赌注');
        return;
      }

      // 总共十二回合，每进行一次，则减少一轮
      // 前九回合
      if(this.roundNum<9){
          this.roundNum++;
          
          // 根据随机数0和1判断是赚是亏
          let random = Math.floor(Math.random() * 2 ) + 0;
          if(random === 0){
            // 此概率下血本无归
             let leftCapital = this.currentCapital - this.inputStake;
             this.revenue = this.revenue + leftCapital
          }else{
            // 此概率下赚大了，2.5倍
              let income = this.inputStake * 2.5;
              // 剩余本金
              let leftCapital = this.currentCapital - this.inputStake;
              this.revenue = this.revenue + income + leftCapital
        
          }

          this.inputStake = '';
      }else if(this.roundNum>8 && this.roundNum<12){
        // 后三回合
        
          this.roundNum++;
          // 平均当前资产作为每次的本金
          if(this.roundNum === 10){
              this.currentCapital = this.revenue / 3;
          }
          
            // 根据随机数0和1判断是赚是亏
          let random = Math.floor(Math.random() * 2 ) + 0;
          if(random === 0){
            // 此概率下血本无归
             let leftCapital = this.currentCapital - this.inputStake;
             this.revenue = this.revenue + leftCapital
          }else{
            // 此概率下赚大了，2.5倍
              let income = this.inputStake * 2.5;
                   // 剩余本金
              let leftCapital = this.currentCapital - this.inputStake;
              this.revenue = this.revenue + income + leftCapital
          }
           this.inputStake = '';
      }else{
        this.disabled = true;
        //return;
      }

     
    },    
    saveInfo(formName){
      this.$refs[formName].validate((valid) => {
          if (valid) {
            this.disabled = false;
            this.$message({
          message: '信息录入成功，请您开始测试',
          type: 'success'
        });
          } else {
            console.log('error submit!!');
            return false;
          }
        });
  
    },
     create_websql() {
        //创建名为mydb的数据库             版本    描述      大小
          this.db = openDatabase('psyDB', '1.0', 'psyDB DB', 2 * 1024 * 1024);
        //创建一个名为BART的表,如果存在则不会创建
          this.db.transaction(function (tx) {
            tx.executeSql('CREATE TABLE IF NOT EXISTS PSYT (id, gender, age, finalGrade,timestamp)');          
          });
      },
  }
}
</script>

<style  rel="stylesheet/scss" lang="scss">
  .example_upload {
    float: left;
  }

  .tips-wrap{
  margin-top: 30px;
  border: 1px solid #eee;
  padding: 20px 40px;
  position: relative;
  background: #edf1f5;
  margin-bottom: 10px;
  .tips-title{
    i{
      position: absolute;
      top: 8px;
      left: 10px;
      width: 16px;
      height: 16px;
      vertical-align: middle;
     // background: url("~@/asset/tips.png") no-repeat 0 0;

    }
    border: 1px solid #ccc;
    background-color: #fff;
    color: #dd4b39;
    position: absolute;
    top: -18px;
    left: 30px;
    padding: 5px 10px 5px 34px;
    font-size: 15px;
  }
}

.cnki-wrap{
  text-align: left;
  box-sizing: border-box;
  overflow: auto;
  background: #edf1f5;
  padding: 5px;

}

</style>
