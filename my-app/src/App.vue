<template>
  <div>
    <el-container
      align="center"
      justify="center"
    >
    <el-header class="header" 
    justify="center" 
    align="center">
      <TheHeader :percentDone="percentDone"
      :countDone="countDone"
      :taskArr="taskArr" />
    </el-header>
    <el-main class="main"> 
      <el-space
        wrap
        style="width:100%"
        direction="vertical"
      >
      <!-- INPUT group -->
        <el-row  class="main-input" :span="12" :offset="6">
          <el-col>
          <el-input
            v-model="task"
            placeholder="create a Vue.js app"
            class="input-with-select"
          >
            <template #prepend >
                <el-time-select
                  v-model="dura"
                  start="00:00"
                  step="00:15"
                  end="24:00"
                  placeholder="00:00"
                  class="time-input"
                  arrow-control
                >
                </el-time-select>
            </template>
            
            <template #append>
              <el-button
              class="add-btn"
              @click="addTask" 
              :disabled="task.length <=0"
              >
                <el-icon color="white">
                  <svg viewBox="0 0 1024 1024" xmlns="http://www.w3.org/2000/svg" data-v-ba633cb8=""><path fill="currentColor" d="M480 480V128a32 32 0 0 1 64 0v352h352a32 32 0 1 1 0 64H544v352a32 32 0 1 1-64 0V544H128a32 32 0 0 1 0-64h352z" data-darkreader-inline-fill="" style="--darkreader-inline-fill:currentColor;"></path></svg>
                </el-icon>
              </el-button>
              
            </template>
          </el-input>
          </el-col>
        </el-row>

      <!-- OUTPUT group -->
        <el-row class="main-output">
          <el-table
            :data="taskArr"      
            :row-class-name="tableRowClassName"
            header-cell-class-name="table-header"
            row-cell-style="{background: 'blue'}" 
            height="30vh"     
          >

            //Duration display
            <el-table-column 
              label="Duration" 
              prop="dura" 
              width="90"
              align="center"
              >     
            </el-table-column>

            //Task display
            <el-table-column 
              label="Task" 
              prop="task" 
              width="300vw"
              align="center"
              >     
            </el-table-column>

            //Checkbox: Done
            <el-table-column 
            width="90"
            label="Done" 
            align="center"
            
            >
              <template #default="scope">                            
                  <el-checkbox
                    v-model="scope.row.done"
                    size="large"  
                    
                    @click.prevent="checkDone(scope.$index, scope.row)"
                  >
                  </el-checkbox>
              </template>
            </el-table-column>

            //Delete btn
            <el-table-column width="100" 
            label="Delete" align="center" 
            >
              <template #header>
                <el-button v-model="search" 
                @click="delAll()" 
                style="background-color: #e0b0ff"
                >
                <el-icon size="25px" color="purple">
                  <svg viewBox="0 0 1024 1024" xmlns="http://www.w3.org/2000/svg" data-v-ba633cb8=""><path fill="currentColor" d="M771.776 794.88A384 384 0 0 1 128 512h64a320 320 0 0 0 555.712 216.448H654.72a32 32 0 1 1 0-64h149.056a32 32 0 0 1 32 32v148.928a32 32 0 1 1-64 0v-50.56zM276.288 295.616h92.992a32 32 0 0 1 0 64H220.16a32 32 0 0 1-32-32V178.56a32 32 0 0 1 64 0v50.56A384 384 0 0 1 896.128 512h-64a320 320 0 0 0-555.776-216.384z" data-darkreader-inline-fill="" style="--darkreader-inline-fill:currentColor;"></path></svg>
                </el-icon>
                </el-button>
              </template>
              <template #default="scope">
                <el-button
                  @click.prevent="delTask(scope.$index)"
                  v-if="!scope.row.done"
                >   
                  <el-icon size="20px" color="purple" >
                    <svg viewBox="0 0 1024 1024" xmlns="http://www.w3.org/2000/svg" data-v-ba633cb8=""><path fill="purple" d="M160 256H96a32 32 0 0 1 0-64h256V95.936a32 32 0 0 1 32-32h256a32 32 0 0 1 32 32V192h256a32 32 0 1 1 0 64h-64v672a32 32 0 0 1-32 32H192a32 32 0 0 1-32-32V256zm448-64v-64H416v64h192zM224 896h576V256H224v640zm192-128a32 32 0 0 1-32-32V416a32 32 0 0 1 64 0v320a32 32 0 0 1-32 32zm192 0a32 32 0 0 1-32-32V416a32 32 0 0 1 64 0v320a32 32 0 0 1-32 32z" data-darkreader-inline-fill="" style="--darkreader-inline-fill:currentColor;"></path></svg>
                  </el-icon>
                </el-button>
              </template>
            </el-table-column>
          </el-table>
        </el-row>     
      </el-space>
    </el-main>
    <el-footer class="footer">
      <TheFooter />
    </el-footer>  
    </el-container>
  </div>
</template>
    
<script>
import { ref, onMounted } from 'vue'
import { ElNotification } from 'element-plus'
import TheHeader from './components/TheHeader.vue'
import TheFooter from './components/TheFooter.vue'

export default {
  name: 'theApp',
  components:{ TheHeader, TheFooter }  ,
  setup(){
    
    const task = ref('')
    const dura = ref('')
    const taskArr = ref([])
    
    const countDone = ref(0) 
    const percentDone = ref(0) 
    onMounted(() => {
      if(localStorage.getItem('tasks')){
      const savedData = JSON.parse(localStorage.getItem('tasks'))
      taskArr.value = savedData
      countDone.value = taskArr.value.map(e => e.done)
                        .filter(e => e == true).length
      percentDone.value = countDone.value/taskArr.value.length * 100
      }
    })

      const delAll = () => {
        console.log('delAll', taskArr.value)
        taskArr.value = []
        countDone.value = 0
        percentDone.value = 0
        dura.value = []
        storeToLocal()
      }

      const addDuraMsg = () => {
        ElNotification({
          type: 'warning',
          title: `Hey dude! How much time for "${task.value}" ?`,
          position: 'bottom-right',
          customClass: 'nofi-msg',
        })
      }
      const dupDataMsg = () => {
        ElNotification({
          title: `Hey dude! "${task.value}" has already been added!`,
          type: 'error',
          position: 'bottom-right',
          customClass: 'nofi-msg',
        })
      }

      const successMsg = () => {
        ElNotification({
          title: 'Task added to your list!',
          message: `"${ task.value }" is to be finished in ${dura.value.substring(0,2)}h ${dura.value.substring(3,5)}min`,
          type: 'success',
          position: 'bottom-right',
          customClass: 'nofi-msg',
        })
      }

    const addTask = () => {
      const extractArr = taskArr.value.map(e => e.task)
      console.log('test extractArr', extractArr, typeof dura.value, dura.value, percentDone.value, taskArr.value.length)
      if(!extractArr.includes(task.value)){
        if(dura.value == "00:00" || dura.value == ''){
          addDuraMsg()
        } else {
          taskArr.value.push({dura: dura.value, task: task.value, done: false})
          successMsg()
        }
      }else{
        dupDataMsg()
      }
      storeToLocal()  
    }

    const delTask = (index) => {
      taskArr.value.splice(index, 1)
      storeToLocal()
    }

    const checkDone = (i, row) => {
      taskArr.value[i].done = !row.done 
      countDone.value = row.done == true ? countDone.value + 1 : countDone.value - 1
      percentDone.value = countDone.value/taskArr.value.length * 100
      storeToLocal()
    }


    const storeToLocal = () => {
      const storageData = JSON.stringify(taskArr.value)
      localStorage.setItem('tasks', storageData)
    }
    
    const tableRowClassName=({row}) => {
    if (row.done === true) {
        return 'done-row';
      }else{
        return 'undone-row';
      }
    }

    return {   
      countDone,
      percentDone,
      dura,
      task,
      taskArr,
      
      tableRowClassName,
      addTask,
      delAll,
      delTask,
      storeToLocal,
      checkDone, 
      successMsg,
      dupDataMsg,
    
    }
  }
}
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Comfortaa&display=swap');

html,body{ 
  padding: 0;
  margin: 0;
  box-sizing: border-box; 
  background-color: #e0b0ff;
  text-align: center;
  font-family: 'Comfortaa', cursive;
   
}

.header {
  width:100%; 
  margin: auto; 
  height:fit-content;
}

.main {
  width:80%; 
  margin:auto; 
  /* height:38vh;  */
  overflow: hidden;
}

.footer {
  width:100%; 
}

/* -- input styles -- */
.time-input {
  width: 110px; 
  background-color: black;
  color: #e0b0ff;
}

.add-btn {
  background-color: black !important;
  border-radius: 0;
}

.add-btn:hover {
  background-color: #7F00FF !important;
  color: #CF9FFF;
  border-radius: 0;
}

.input-with-select {
  background-color: black !important;
  padding: 2px;
  border-radius: 5px;
  width: 370px;
  font-family: 'Comfortaa'!important;
}

/* -- table styles -- */

.el-table {
  border-radius: 5px ;
  margin: 5px;
  border: 2px inset #7b68ee;
  color: purple
}

.table-header {
  background: black !important; 
  color: #e0b0ff;
}

.el-table .done-row {
  text-decoration: line-through;
  background-color: #f4bbff;
  color: purple !important;
}

.el-checkbox-button {
  color: #7b68ee;
  background-color: #7b68ee;
}

.nofi-msg{
  background-color: #ffcff1;
  border: #ff1493 dashed 1px;
}

</style>