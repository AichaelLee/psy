<template>
  <div>
    <el-row>
       <!-- 信息栏 begin -->
      <el-row :gutter="10">
        <el-form :model="ruleForm" :rules="rules" ref="ruleForm" label-width="0px" class="demo-ruleForm">
  
        <el-col :xs="12" :sm="12" :md="3" :lg="3" :xl="3" :offset="1" style="margin-top:10px">
           
           <el-button type="info"  @click="openNotice" plain round>指导语</el-button>
        </el-col>
         <el-col :xs="12" :sm="12" :md="4" :lg="4" :xl="4" style="margin-top:10px">
            <el-card class="box-card" style="height:38px;padding-top:0px" >
            受试者信息管理
          </el-card>
        </el-col>
      <el-col :offset="1" :xs="8" :sm="8" :md="4" :lg="4" :xl="4" style="margin-top:10px">
        <el-form-item  prop="id">
        <el-input
          placeholder="序号"
          v-model.number="ruleForm.id"
          clearable
          :disabled="noDisabled">
        </el-input>
        </el-form-item>
        </el-col>
        <el-col :xs="6" :sm="6" :md="2" :lg="2" :xl="2" style="margin-top:10px">
           <el-form-item  prop="gender">
            <el-select v-model="ruleForm.gender" clearable placeholder="性别" :disabled="genderDisabled">
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
          clearable
          :disabled="ageDisabled">
        </el-input>
           </el-form-item>
        </el-col>
        <el-col :xs="14" :sm="14" :md="3" :lg="3" :xl="3" style="margin-top:10px">
           <el-button v-if="!ifcanFix" type="primary" plain @click="saveInfo('ruleForm')" :disabled="ifcanFix">开始</el-button>
           <el-button v-if="ifcanFix" type="primary" plain @click="modifiedInfo" :disabled="!ifcanFix">修改</el-button>
           
        </el-col>
             <el-col :xs="6" :sm="6" :md="3" :lg="3" :xl="3" style="margin-top:10px">
           <el-button type="primart" @click="statis">统计</el-button>
           
        </el-col>
       
        </el-form>
    </el-row>
      <!-- 信息栏 end -->
      <br><br>
    </el-row>

      <!-- </el-col> -->
      <!-- 气球 -->
      <el-col :xs="24" :sm="24" :md="12" :lg="12" :xl="12" class="balloonPosition">
        <el-row>
          <el-col class="imgPosi" :xs="24" :sm="24" :md="24" :lg="24" :xl="24" :style="{ 'margin-top':magrintop+'px'}">
            <!-- <div id="div1" @click="clickBom"></div> -->
          
            <el-row>
               <el-col :xs="22" :sm="22" :md="22" :lg="22" :xl="22" :offset="2"> 
                 <img src="./balloon.png" class="ballimg" :style="{ height: balloonHeight+'px', width: balloonWidth + 'px' }" @click="clickBalloon" ></el-col>
                    <audio controls id="MyAudio2" hidden>
                          <source src="./explode.mp3" type="audio/mpeg"> 
                          您的浏览器不支持该音频格式。
                      </audio>
                       <audio controls id="getRewordMusic" hidden>
                          <source src="./collect.mp3" type="audio/mpeg"> 
                          您的浏览器不支持该音频格式。
                      </audio>
            </el-row>
            
          </el-col>
        </el-row>
           
        
        </el-col>
      <!-- 信息 -->
      <el-col :xs="22" :sm="22" :md="4" :lg="4" :xl="4" :offset="2" style="margin-top:20px">
        <el-row>
          <el-col :xs="24" :sm="24" :md="24" :lg="24" :xl="24">
           <el-row>
             <el-col :xs="12" :sm="12" :md="14" :lg="14" :xl="14">上一个气球收益</el-col>
             <el-col :xs="12" :sm="12" :md="10" :lg="10" :xl="10" style="height:50px;color:red">￥&nbsp;{{lastTimeScore*2*1/100}}</el-col>
           </el-row>
          </el-col>
        </el-row>
            <el-row style="margin-top:15px">
          <el-col :xs="24" :sm="24" :md="24" :lg="24" :xl="24">
            <el-row>
             <el-col :xs="12" :sm="12" :md="14" :lg="14" :xl="14">总收益&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</el-col>
             <el-col :xs="12" :sm="12" :md="10" :lg="10" :xl="10" style="height:50px;color:red">￥&nbsp;{{sumScore*2*1/100}}</el-col>
           </el-row>
          </el-col>
        </el-row>
         <!-- <el-row style="margin-top:15px">
          <el-col :xs="24" :sm="24" :md="24" :lg="24" :xl="24">
            <el-row>
             <el-col :xs="12" :sm="12" :md="12" :lg="12" :xl="12">未爆破气球总数</el-col>
             <el-col :xs="12" :sm="12" :md="12" :lg="12" :xl="12" style="height:50px;color:red">&nbsp;{{doneUnboomend}}</el-col>
           </el-row>
          </el-col>
        </el-row> -->
            <el-row>
          <el-col :xs="12" :sm="12" :md="16" :lg="16" :xl="16" class="leftTime" style="margin-top:30px">
            游戏还剩<span class="singleFont">{{leftTime}}</span>局
          </el-col>
          <el-col :xs="12" :sm="12" :md="24" :lg="24" :xl="24">
            <el-row style="margin-top:20px">
          <el-col :xs="12" :sm="12" :md="16" :lg="16" :xl="16">
            <el-button type="primary" @click="getReward" plain :disabled="ifDisabled">获得奖励</el-button>
          </el-col>
        </el-row>
          </el-col>
        </el-row>
        
        
      </el-col>
  
   
    
    <!-- <el-row>
      <el-col :xs="8" :sm="8" :md="8" :lg="8" :xl="8">总分:</el-col>
      <el-col :offset="8" :xs="8" :sm="8" :md="8" :lg="8" :xl="8"> {{sumScore}}分</el-col></el-row>
     <el-row>
       <el-col :xs="8" :sm="8" :md="8" :lg="8" :xl="8">第{{bumNum}}个气球</el-col>
       <el-col :offset="8" :xs="8" :sm="8" :md="8" :lg="8" :xl="8">充了{{times}}下气</el-col>
       </el-row>
       < 这里是气球
       <el-row>
         <div id="div1"></div>
       </el-row>
</el-row> -->
  </div>
</template>
<script>
export default {
  created(){
    this.ruleForm.id=this.$route.params.id
    this.ruleForm.gender=this.$route.params.gender
    this.ruleForm.age = this.$route.params.age

    this.infoDone = !!this.$route.params.id
    this.noDisabled = !!this.$route.params.id
    this.ageDisabled= !! this.$route.params.age
    this.genderDisabled = !!this.$route.params.gender
   
    var that=this;
			document.onkeydown=function(e){
        var key=window.event.keyCode;
				if(key==49){
					that.clickBalloon();
				}else if(key==53){
          that.getReward();
        }
      }
      // 创建web数据库
      this.create_websql()

  },
   mounted(){
    this.over = this.RandomNum()
  },
    data () {
    return {
      ruleForm:{
        id:'',
        gender:'',
        age:''
      },
      infoDone:false,
      noDisabled:false,
      ageDisabled:false,
      genderDisabled:false,
      name:'',
      times: 0,
      initialScore :0,
      sumScore: 0,
      lastTimeScore:0,
      // 结束后未爆破的次数
      doneUnboomend:'',
      unBoomed:0,
      // 点击气球的总数
      clickTimes:0,
      bumNum:0,
       options: [{
          value: '1',
          label: '男'
        }, {
          value: '2',
          label: '女'
        }
        ],
        value4: '',
        age:'',
        leftTime:30,
        // 气球的高度和宽度
        balloonHeight:40,
        balloonWidth:40,
        magrintop:200,
        over:'',
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
    
  computed:{
    ifDisabled(){
      return this.initialScore===0;
    },

    balloonSize(){
      return{
        'height':`${this.balloonHeight}px`,
        'width' :`${this.balloonWidth}px`
      }

      
    },
  
    ifcanFix(){
      return this.noDisabled
      }
  },
  watch:{
    leftTime(newV,oldV){
      if(newV===0){
        this.doneUnboomend = this.unBoomed
            // 最后一个气球已经获得奖励了,insert db
            let _this = this
            this.db.transaction(function (tx) {
            tx.executeSql('INSERT INTO BART (id, gender, age, sumScore,unBoomedNum,timestamp) VALUES (?, ?,?, ?, ?,?)',[_this.ruleForm.id,_this.ruleForm.gender, _this.ruleForm.age, _this.sumScore*2*1/100,_this.unBoomed,new Date()]);
            
             });

      }

    }
  },
 
  methods:{
    openNotice(){
           this.$alert('气球模拟决策任务的实验，.......', '气球模拟决策任务', {
          confirmButtonText: '确定',
          callback: action => {
            this.$message({
              type: 'success',
              message: '已经知悉'
            });
          }
        });
    },
    modifiedInfo(){
      this.noDisabled = false;
      this.ageDisabled= false;
      this.genderDisabled=false
    },
    statis(){this.$router.push({ path:  '/excel' })},
    saveInfo(formName){
      this.$refs[formName].validate((valid) => {
          if (valid) {
            this.infoDone = true;
            this.noDisabled = true
            this.ageDisabled= true
            this.genderDisabled = true
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
    clickBalloon(){
      if(this.infoDone){
         if(this.leftTime>0){
            this.magrintop-=2;
            this.balloonWidth+=2;
            this.balloonHeight+=2;
            this.initialScore+=1;
            this.clickTimes+=1;
            if(this.initialScore<=this.over){
              // 未爆炸
            }else{
              document.getElementById('MyAudio2').play()
             
              
              // 已经爆炸了
              //恢复气球到初始大小
               this.balloonWidth=40;
               this.balloonHeight=40;
               this.magrintop=200;
               //显示上次收益：
               //this.lastTimeScore = this.clickTimes
               // 重置本次得分
              this.initialScore = 0;
              this.lastTimeScore = 0;
              this.clickTimes = 0
              this.hadBoomed();
              this.leftTime-=1;
              this.over = this.RandomNum()

            }
      }

      }
      else{
        this.$message.error('请您输入个人信息!');
      }
     
    
    },
     // 获得奖励
    getReward(){
        if(this.infoDone){
              //操作建立在剩余测试数大于0的情况
            if(this.leftTime>0&&this.clickTimes>0){
              // 未爆破数目
              this.unBoomed += 1;
          // 增加总分数
          // this.sumScore += this.initialScore;
          this.sumScore = this.add(this.initialScore,this.sumScore);
          // 把本次得分改为0
          this.initialScore = 0;
          // 测试次数减少1
          this.leftTime -= 1;
          //显示上次收益：
          this.lastTimeScore = this.clickTimes
          this.clickTimes = 0
          // 恢复原始大小
          this.balloonWidth=40;
          this.balloonHeight=40;
          this.magrintop=200;
           document.getElementById('getRewordMusic').play()

      }
        }else{
          this.$message.error('请您输入个人信息!');
        }
  
     

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
     create_websql() {
        //创建名为mydb的数据库             版本    描述      大小
          this.db = openDatabase('bartdb', '1.0', 'bartdb DB', 2 * 1024 * 1024);
        //创建一个名为BART的表,如果存在则不会创建
          this.db.transaction(function (tx) {
            tx.executeSql('CREATE TABLE IF NOT EXISTS BART (id, gender, age, sumScore,unBoomedNum,timestamp)');          
          });
      },
    hadBoomed() {
        this.$message({
          showClose: true,
          message: '气球爆炸啦！',
          center: true,
          type: 'warning',
          duration:'2000'
        });
      },
      RandomNum() {
            let Min = 0;
            let Max = 128;
             var Range = Max - Min;
             var Rand = Math.random();
             if(Math.round(Rand * Range)==0){             
                 return Min + 1;
             }
             var num = Min + Math.round(Rand * Range);
             return num;
       }
  }
}
</script>

<style type="text/css">
.imgPosi{
  margin: auto
}
.el-card__body{
  padding:7px !important;
}
.scoreSize{
  height: 50px;
  width: 40px;
}

.leftTime{
  color:rgb(6, 30, 136);
  font-size:110%
}
.singleFont{
  color: rgb(179, 20, 20) !important;
  padding:5px;
  font-size: 120%;
}
.balloonPosition{
  text-align:center
}
/* .ballimg {
  position: absolute;
  left: 30px;
  top: 20px;
  bottom: 0px;
} */
        #div1{
          
            width:100px;
            height:100px;
            border-radius: 50%;
            background-color: skyblue;
           /* background-image: url('qq.jpg');*/
        }
        .proHeight{
          height: 120px;
          width: 70px;
          margin:0px;
        }
        .colHeight{
          height: 100px !important;
          width: 35px !important;
        }
        .box-card1{
          background-color: rgb(141, 3, 3);
          height: 100%;
      
        }
         .box-card2{
          background-color: rgb(92, 21, 139);
          height: 100%;
        }
         .box-card3{
          background-color: rgb(36, 46, 184);
          height: 100%;
        } 
        .box-card4{
          background-color: rgb(18, 105, 145);
          height: 100%;
        }
    </style>

