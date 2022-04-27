<template>
   <div class="wrapper">
      <header>
         <div class="container">
            <h1>Список дел</h1>
         </div>
      </header>
      <div class="container">
         <div class="form">
            <div class="add-task">Добавить задачу</div>
            <input
               type="text"
               :value="valueInput"
               @input="handlyInput"
               @keypress.enter="addTask"
            />
            <button class="btn" @click="addTask">Добавить</button>
         </div>

         <h2>
            <span class="subtitle">Нужно сделать</span>
            <span class="tasks-number">{{ needDoList.length }}</span>
         </h2>
         <ul class="task-list">
            <li class="task" v-for="(task, index) in needDoList" :key="task.id">
               <div>
                  <input
                     class="checkbox"
                     type="checkbox"
                     @change="doCheck(index, 'need')"
                  />
                  <div class="task-text">
                     {{ task.text }}
                  </div>
               </div>
               <button class="btn-remove" @click="removeTask(index, 'need')">
                  Удалить
               </button>
            </li>
         </ul>
         <button class="btn btn-clear" @click="clearList('need')">
            Очистить список
         </button>

         <h2>
            <span class="subtitle">Выполненные задачи</span>
            <span class="tasks-number">{{ completeList.length }}</span>
         </h2>
         <ul class="task-list">
            <li
               class="task"
               v-for="(task, index) in completeList"
               :key="task.id"
            >
               <div>
                  <input
                     class="checkbox"
                     type="checkbox"
                     @change="doCheck(index, 'complete')"
                     checked
                  />
                  <div class="task-text">{{ task.text }}</div>
               </div>
               <button
                  class="btn-remove"
                  @click="removeTask(index, 'complete')"
               >
                  Удалить
               </button>
            </li>
         </ul>
         <button class="btn btn-clear" @click="clearList('complete')">
            Очистить список
         </button>
      </div>
   </div>
</template>

<script>
export default {
   data() {
      return {
         inputValue: "",
         needDoList: [],
         completeList: [],
      };
   },
   methods: {
      handlyInput(event) {
         this.inputValue = event.target.value;
      },
      addTask() {
         if (this.inputValue === "") {
            return;
         }
         this.needDoList.push({
            text: this.inputValue,
            id: Date.now(),
         });
         this.inputValue = "";
      },
      doCheck(index, type) {
         if (type === "need") {
            let task = this.needDoList.splice(index, 1);
            this.completeList.unshift(...task);
         }
         if (type === "complete") {
            let task = this.completeList.splice(index, 1);
            this.needDoList.push(...task);
         }
      },
      removeTask(index, type) {
         type === "need"
            ? this.needDoList.splice(index, 1)
            : this.completeList.splice(index, 1);
      },
      clearList(type) {
         type === "need"
            ? this.needDoList.splice(0)
            : this.completeList.splice(0);
      },
   },
};
</script>

<style lang="scss">
@import "src/reset";
* {
   box-sizing: border-box;
}
header {
   width: 100%;
   background-color: rgb(92, 27, 27);
   margin-bottom: 5px;
}
h1 {
   font-size: 24px;
   text-align: center;
   padding: 15px;
   color: #ffffff;
}
h2 {
   display: flex;
   align-items: center;
   justify-content: space-between;
   padding-bottom: 20px;
   margin: 25px 0;
   border-bottom: 1px solid #bfbfbf;
}
.container {
   width: 800px;
   margin: 0 auto;
   padding: 0 15px;
}

.form {
   display: flex;
   align-items: center;
   justify-content: space-between;
   padding: 10px 0;
   input {
      width: 65%;
      height: 40px;
      text-indent: 10px;
      border: 2px solid rgb(92, 27, 27);
      transition: 0.2s border;
      &:focus {
         border: 2px solid rgb(234, 79, 79);
      }
   }
}
.add-task {
   font-size: 18px;
}
.btn {
   background-color: #029f2c;
   border: 1px solid #029f2c;
   color: #fff;
   font-weight: 700;
   text-transform: uppercase;
   padding: 10px 15px;
   font-size: 12px;
   height: 42px;
   outline: none;
   transition: 0.2s;
   &:hover {
      background-color: #005216;
      border-color: #005216;
   }
}

.subtitle {
   font-size: 20px;
   font-weight: 600;
}
.tasks-number {
   border-radius: 22px;
   border: 2px solid rgb(92, 27, 27);
   color: rgb(92, 27, 27);
   font-size: 16px;
   font-weight: 700;
   padding: 5px 10px;
}
.task-list {
}
.task {
   display: flex;
   justify-content: space-between;
   margin-bottom: 15px;
   div {
      display: flex;
   }
}
.checkbox {
   display: block;
   margin-right: 10px;
   cursor: pointer;
   transform: scale(1.4);
}
.task-text {
   font-size: 18px;
   width: 96%;
   word-break: break-word;
}
.btn-remove {
   background-color: rgb(92, 27, 27);
   border: 1px solid rgb(92, 27, 27);
   color: #fff;
   font-weight: 700;
   text-transform: uppercase;
   border-radius: 18px;
   padding: 6px 10px;
   font-size: 12px;
   height: 28px;
   outline: none;
   transition: 0.2s;
   &:hover {
      background-color: rgb(183, 58, 58);
      border-color: rgb(183, 58, 58);
   }
}
.btn-clear {
   display: block;
   margin-left: auto;
   background-color: rgb(92, 27, 27);
   border: 1px solid rgb(92, 27, 27);
   transition: 0.2s;
   &:hover {
      background-color: rgb(183, 58, 58);
      border-color: rgb(183, 58, 58);
   }
}
</style>
