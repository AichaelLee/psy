<template>
  <!-- <div>
    <cnki-upload ref='uploader' class="example_upload" pickId="#test" pickText="选择文件"></cnki-upload>
    <div class='example_upload' id="test"></div>
    <el-button @click="test">你要做什么操作啊</el-button>
 
  </div> -->
  <div>
    <el-row>

      <el-col :span="24" >
        
        <el-dialog
         
          :visible.sync="dialogVisible"
          width="90%"
    
          :before-close="handleClose">
         

          <div slot="title" class="header-title">
            <span  style="font-size:25px" class="title-name">指导语</span>
          </div>

         
            <el-row> 
              <div style="height:500px">
              <el-col :span="16" :offset="4">

                <span style="font-size:22px;line-height:50px;letter-spacing:5px">
                   &nbsp;<br> &nbsp; &nbsp;<br> 
                  欢迎来到决策游戏，整个实验大约持续15分钟，在游戏结束之后，你将会拿到金额与你游戏总分相关的实验报酬。
                  接下来的实验分为2部分，下面是第1部分的指导语：
                  在这一部分，你将看到9个独立回合的投资游戏，每次你将
                  收到20分，你必须选择投入一部分X（在0和20之间），对于你投入的那部分X，1/2的可能会损失X，1/2的可能会得到2.5X。
                  请你做出自己的选择。 &nbsp;<br> &nbsp;<br> &nbsp;<br> &nbsp;<br> &nbsp;<br>
                 &nbsp;<br> &nbsp;<br> &nbsp;<br>
                 </span>
              </el-col>    
              </div>
            </el-row>
 
      
         
          <span slot="footer" class="dialog-footer">
            <el-button @click.native="dialogVisible = false">取 消</el-button>
            <el-button type="primary" @click.native="dialogVisible = false">确 定</el-button>
          </span>
       
         
        </el-dialog>
      </el-col>



      <!-- 第二轮指导语 -->
      <el-col :span="24">
        <el-dialog
          :visible.sync="round2Dialogue"
          width="70%"
          :before-close="handleClose">
           <div slot="title" class="header-title">
            <span  style="font-size:25px" class="title-name">指导语</span>
        </div>
          <span  style="font-size:22px">
            这是第二部分的指导语
          </span>
          <span slot="footer" class="dialog-footer">
            <el-button @click="round2Dialogue = false">取 消</el-button>
            <el-button type="primary" @click="round2Dialogue = false">确 定</el-button>
          </span>
        </el-dialog>
      </el-col>
      
    </el-row>
    <el-row>
      <el-col :xs="24" :sm="24" :md="24" :lg="24" :xl="24">

    <div>

    <div class="cnki-wrap">
 <!-- 顶头栏信息 -->
            <el-row :gutter="7">
             <el-form :model="ruleForm" :rules="rules" ref="ruleForm" label-width="0px" class="demo-ruleForm">
                <el-col :offset="1" :xs="6" :sm="4" :md="4" :lg="4" :xl="4" style="margin-top:10px">
                    <el-form-item  prop="id">
                    <el-input
                      placeholder="序号"
                      v-model.number="ruleForm.id"
                      clearable>
                    </el-input>
                    </el-form-item>
                </el-col>

                <el-col :xs="6" :sm="4" :md="2" :lg="2" :xl="2" style="margin-top:10px">
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

               <el-col :xs="6" :sm="4" :md="2" :lg="2" :xl="2" style="margin-top:10px">
                      <el-form-item  prop="age">
                      <el-input
                      placeholder="年龄"
                      v-model.number="ruleForm.age"
                      clearable>
                    </el-input>
                      </el-form-item>
                </el-col>
               
                <el-col :offset="1" :xs="6" :sm="3" :md="3" :lg="3" :xl="3" style="margin-top:10px">
                      <el-button type="primary" plain @click="saveInfo('ruleForm')" >开始</el-button>
               
                </el-col>
                <el-col :xs="6" :sm="3" :md="3" :lg="3" :xl="3" style="margin-top:10px">
                     
                       <el-button type="primart" @click="statis">统计</el-button>
                </el-col>

               </el-form>
                      
            </el-row>


       </div>

      <div class="tips-wrap">
        <div class="tips-title">
          <i></i>
          <span>实验提示</span>
        </div>
        <el-row style=" text-align:left">
          <el-col :xs="24" :sm="24" :md="24" :lg="24" :xl="24">

             本回合你拥有<span style="color:#dd4b39; font-weight:bold; font-size:16px; text-align:left">{{this.currentCapital}}</span>分，请选择投入 <br><br>
            对于你投入的这部分，1/2的可能会损失，1/2的可能会扩大2.5倍。<br><br> 
               
              按Enter键进入下一回合    
            
          </el-col>
        </el-row>
       
       </div>

        <div >
          <div class="cnki-wrap">

         

             <el-row>
                <el-col :xs="12" :sm="12" :md="12" :lg="12" :xl="12" :offset="1" style="margin-top:18px">
                    已进行回合数:{{roundNum}}
                    <!-- <span v-if="roundNum === 12">
                      当前结果 ：{{revenue}}
                    </span> -->当前结果 ：{{revenue}}
                </el-col>
              </el-row>
                
              <el-row style="margin-top:18px;margin-bottom:18px">
                <el-col :xs="6" :sm="6" :md="6" :lg="6" :xl="6" :offset="1">
                    <el-input size="small" :disabled="disabled" v-model="inputStake" placeholder="请输入赌注" @keyup.enter.native="nextRound"></el-input>
                </el-col>
                  <el-col :xs="3" :sm="3" :md="3" :lg="3" :xl="3" :offset="1">
                    <el-button size="small" round @click="nextRound" type="primary" :disabled = "disabled">下一回合</el-button>
                </el-col>
              </el-row>

              <!-- <el-row>
                <el-col :span="12" :offset="1" style="margin-top:18px">
                    <el-button size="small" round @click="nextRound" type="primary" :disabled = "disabled">下一回合</el-button>
                </el-col>
              </el-row> -->

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
        dialogVisible:true,
        round2Dialogue:false,
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
        cuin:'',
        // 回合总数
        roundNum:0,
        // 投入赌注：
        inputStake:'',
        // 当前可支配的本金
        currentCapital:20,
        // 后三个可支配的本金
        last3Capital:'',
        // 每一回合的分数
        allRoundGrade:[],

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
    //this.openNotice();
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
            console.log("database:+"+_this.ruleForm.id,_this.ruleForm.gender, _this.ruleForm.age, _this.revenue,new Date())
            tx.executeSql('INSERT INTO PSYT (id, gender, age,r1,r2,r3,r4,r5,r6,r7,r8,r9,r10,r11,r12,finalGrade,timestamp) VALUES (?, ?,?, ?, ?,?,?,?,?,?,?,?,?,?,?,?,?)',
            [_this.ruleForm.id,_this.ruleForm.gender, _this.ruleForm.age,
            _this.allRoundGrade[0],
            _this.allRoundGrade[1],
            _this.allRoundGrade[2],
            _this.allRoundGrade[3],
            _this.allRoundGrade[4],
            _this.allRoundGrade[5],
            _this.allRoundGrade[6],
            _this.allRoundGrade[7],
            _this.allRoundGrade[8],
            _this.allRoundGrade[9],
            _this.allRoundGrade[10],
            _this.allRoundGrade[11],
            

            _this.revenue,new Date()]);
            
             });
      }
    }

  },
  methods: {
    test () {
      // 这是有用的数据
      console.log(this.$refs.uploader.fileList)
    },
    statis(){this.$router.push({ path:  '/excel' })},
    // 下一回合
    nextRound(){
   
      if(this.inputStake === ''){
        this.$message.error('请输入赌注');
        return;
      }

      this.allRoundGrade.push(this.inputStake);
      // 总共十二回合，每进行一次，则减少一轮
      // 前九回合
      if(this.roundNum<9){
          this.roundNum++;

      
      
                let num = Math.random();
                num = Math.ceil(num * 100000);
           
          
          // 根据随机数0和1判断是赚是亏
          let random = Math.floor(Math.random() * 2 ) + 0;
          if(num%2 != 0){
            // 此概率下血本无归
             let leftCapital = this.currentCapital - this.inputStake;
             this.revenue = this.add(this.revenue,leftCapital);
          }else{
            // 此概率下赚大了，2.5倍
              let income = this.inputStake * 2.5;
              // 剩余本金
              let leftCapital = this.currentCapital - this.inputStake;
              let sumLeft = this.add(income,leftCapital);
              this.revenue = this.add(this.revenue,sumLeft);
        
          }

          this.inputStake = '';
                     // 第二轮展示第二个指导语
          if(this.roundNum === 9){
               this.round2Dialogue = true;
               // 平均当前资产作为每次的本金
              //this.currentCapital = this.revenue / 3;
              this.currentCapital = this.numDiv(this.revenue,3);
          
             
          }
      }else if(this.roundNum>8 && this.roundNum<12){
        // 后三回合
        
          this.roundNum++;
      
         let num = Math.random();
                num = Math.ceil(num * 100000);
    
         
            // 根据随机数0和1判断是赚是亏
           let random = Math.floor(Math.random() * 2 ) + 0;
          if(num%2 != 0){
            // 此概率下血本无归
             let leftCapital = this.currentCapital - this.inputStake;
              this.revenue = this.add(this.revenue,leftCapital);
          }else{
    
            // 此概率下赚大了，2.5倍
              let income = this.inputStake * 2.5;
              this.cuin = this.inputStake * 2.5;
                   // 剩余本金
              let leftCapital = this.currentCapital - this.inputStake;
              let sumLeft = this.add(income,leftCapital);
              this.revenue = this.add(this.revenue,sumLeft);
          }
           this.inputStake = '';
           if(this.roundNum == 12){
             this.disabled = true;
             this.$message({
                  message: '实验已完成，谢谢您的参与！',
                  type: 'warning'
                });
           }
      }else{
        this.disabled = true;
         this.$message({
          message: '实验已完成，谢谢您的参与！',
          type: 'warning'
        });
        return;
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
            tx.executeSql('CREATE TABLE IF NOT EXISTS PSYT (id, gender, age, r1,r2,r3,r4,r5,r6,r7,r8,r9,r10,r11,r12,finalGrade,timestamp)');          
          });
      },
       // 解决小数相加损失精度的问题
    add(arg1, arg2){
          var r1, r2, m, c;
          try {
            r1 = arg1.toString().split(".")[1].length;
          }
          catch (e) {
            r1 = 0;
          }
          try {
            r2 = arg2.toString().split(".")[1].length;
          }
          catch (e) {
            r2 = 0;
          }
          c = Math.abs(r1 - r2);
          m = Math.pow(10, Math.max(r1, r2));
          if (c > 0) {
            var cm = Math.pow(10, c);
            if (r1 > r2) {
              arg1 = Number(arg1.toString().replace(".", ""));
              arg2 = Number(arg2.toString().replace(".", "")) * cm;
            } else {
              arg1 = Number(arg1.toString().replace(".", "")) * cm;
              arg2 = Number(arg2.toString().replace(".", ""));
            }
          } else {
            arg1 = Number(arg1.toString().replace(".", ""));
            arg2 = Number(arg2.toString().replace(".", ""));
          }
          return (arg1 + arg2) / m;
},

    /** 
    * 除法运算，避免数据相除小数点后产生多位数和计算精度损失。 
    * 
    * @param num1被除数 | num2除数 
    */ 
    numDiv(num1, num2) { 
       return (num1 / num2).toPrecision(3);
    },

       handleClose(done) {
        this.$confirm('确认关闭？')
          .then(_ => {
            done();
          })
          .catch(_ => {});
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
      background: url("~@/asset/tips.png") no-repeat 0 0;

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
