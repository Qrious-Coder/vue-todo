<template>
  <div style="margin: 0 auto">
      <el-row>
        <el-col class="notice" v-if="taskArr.length == 0">
          <p>Hello! Let's add something to do now :D!</p>
        </el-col>
        <el-col class="notice" v-if="percentDone ==0 && taskArr.length >0">
          <p v-if="taskArr.length == 1">Yay! <span class="num-style">{{taskArr.length}}</span> task has added to your to-do list!</p>
          <p v-if="taskArr.length > 1">Yay! <span class="num-style">{{taskArr.length}}</span> tasks have added to your to-do list!</p>
        </el-col>
        <el-col class="notice" v-if="percentDone !== 0 && percentDone < 100">
          <p>Yay! <span class="num-style">{{countDone}}</span> done! 
         <span class="num-style">{{taskArr.length-countDone}}</span> more to go!</p>
        </el-col>
        <el-col class="notice" v-if="percentDone >= 100">
          <p>Awesome! <span class="num-style">100%</span> finished!</p>
        </el-col>
        <el-col :span="12" :offset="6">
          <el-progress type="dashboard" 
            :percentage="percentDone.toFixed(0)"
            color="#7b68ee"
            width="270"
            stroke-linecap="round"
            style="margin-top: 5px"
            >
            <template #default="{ percentage }">
              <span class="percentage-label" v-if="countDone == 0 || taskArr.length == 0">   
                <span><img :src="shakespeare" width="180" height="180" ></span>
              </span>  
              <span class="percentage-label" v-else-if="percentage <99">
                <span><img :src="knight" width="190" height="190" ></span>
                <span class="percentage-value">{{ percentage }}%</span>
              </span>
              <span class="percentage-label" v-else-if="percentage >99">
                <span><img :src="trophy" width="150" height="150"></span>
                <span class="percentage-value">{{ percentage }}%</span>
              </span>                  
            </template>
          </el-progress>
        </el-col>
      </el-row>
      <el-row class="app-title"> 
        <el-col :span="24">
           <h1>To DO or not to DO</h1>
            <img :src="feather" alt="feather" width="50" height="50"> 
        </el-col> 
      </el-row>   
    <!-- header -->
  </div>
</template>
<script>
import knight from '../assets/knight.gif'
import trophy from '../assets/trophy.gif'
import shakespeare from '../assets/shakespeare.png'
import feather from '../assets/feather.png'

export default {
  name: "TheHeader",
  props: [
    'percentDone',
    'countDone',
    'taskArr'
  ],
  setup() {
    return{
      knight,
      trophy,
      shakespeare,
      feather,
    }
  }
 
}
</script>
<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Fredoka+One&family=Modak&display=swap');

.app-title {
  font-family: 'Fredoka One', cursive;
  max-height: fit-content;
} 
.num-style {
  font-family: 'Modak', cursive;
  font-size: 32px;
  color: #9400d3;
}
.notice {
  background-color: #f1a7fe;;
  color: #7b68ee;
  font-family: 'Fredoka One', cursive;
}
.percentage-value {
  display: block;
  font-family: 'Fredoka One', cursive;
  font-size: 1.4rem;
  color: #7b68ee !important;
  margin: 0;
}
.percentage-label {
  font-size: 1.4rem;
}

</style>