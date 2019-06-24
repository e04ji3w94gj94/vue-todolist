<template>
  <div id="app">

    <div class="header">
      <span class="header__title">My Todo</span>
      <!--  綁定監聽事件  v-on的簡短寫法為 @ -->
       <button class="header__add-todo-btn" v-on:click="isDialogVisible = true">
         <span class="mdi mdi-plus"></span>
       </button>
    </div>

    <div class="todo-list">
      <!-- key(唯一)讓compiler更快速地進行頁面渲染 -->
      <!--  綁定資料來源  v-bin: 的簡短寫法為 : -->
      <div v-for="(todo, index) in todos" :key="`todo-${index}`" class="todo-list__item">
        <input  
            class="todo-item__checkbox" 
            type="checkbox" 
            :id="`cb-${index}`" 
            :checked="todo.isCompleted"
            @change="changeTodoStatus(index, $event.target.checked)"
        >
        <label :for="`cb-${index}`">
          <span class="mdi" :class="{'mdi-checkbox-blank-circle-outline': !todo.isCompleted, 'mdi-checkbox-marked-circle':todo.isCompleted}"></span>
        </label>
        <span class="todo-item__text">{{ todo.text }}</span>

        <button class="todo-item__delete-btn" @click="removeTodo(index)">
          <span class="mdi mdi-delete"></span>
        </button>
      </div>
      
    </div>

    <div v-if="isDialogVisible" class="dialog">
      <div class="dialog__card">
        
        <input v-model="newTodo" class="dialog__input" type="text" placeholder="請輸入代辦事項">
        <button class="dialog__confirm-btn" @click="addTodo">確認</button>
        <button class="dialog__cancle-btn" @click="isDialogVisible = false">取消</button>
      </div>
    </div>

  </div>
</template>

<script>


export default {
  name: 'app',
  data() { 
    return  {
      // JSON.parse():將json字串轉換成js的物件
      todos: JSON.parse(localStorage.getItem('todos')) || [],
      isDialogVisible: false,
      newTodo:''
    }

  },
  methods: {
    saveToLocalStorge() {

      localStorage.setItem('todos', JSON.stringify(this.todos))
    },
    addTodo() {
      this.todos.push({
        text: this.newTodo,
        isDialogVisible: false,
        isCompleted: false
      })

      this.newTodo = ''

      this.isDialogVisible = false

      this.saveToLocalStorge()
    },
    removeTodo(index)  {
      // 取代的位置，取代幾個
      this.todos.splice(index, 1)

      this.saveToLocalStorge()
    },
    changeTodoStatus(index, newStatus) {
      this.todos[index].isCompleted = newStatus

      this.saveToLocalStorge()
    }
  }
}
</script>

<style>

/* 為了讓瀏覽器預設值歸0 */
* {
  margin: 0;
  padding: 0;
  line-height: 1;
  box-sizing: border-box;
}
.header{
    height: 120px;
    background-color: #2196f3;
    display: flex; /* 常見的4種 block(預設，把同列其他空間補滿) inline block-inline flex */
    align-items: center;
    padding: 0 24px;
}

.header__title{
  color: #ffffff;
  font-size: 80px;
  font-weight: 500; /* 字體粗細 */
}
.header__add-todo-btn{
  background-color: transparent;
  border: 2px solid #ffffff;
  border-radius: 20px;
  padding: 8px 16px; /* 上下 左右 */
  color: #ffffff;
  font-size: 24px;
  font-weight: 600;
  outline: 0;
  margin: auto 0 auto auto;/* 4種方式設定參數: 1. 1個值(上下左右) 2. 2個值(上下 左右) 3. 4個值(上右下左) */
}
/* active 選擇被按下的按鈕 */
.header__add-todo-btn:active{
  transform: scale(0.95);
}

.dialog{
  display: flex;
  align-items: center; /* 垂直置中 */
  justify-content: center; /* 水平置中 */
  position: absolute;
  top: 0;
  width: 100vw; /*vw:視窗寬的比例*/
  height: 100vh; /*vw:視窗高的比例*/
  background-color: rgba(0, 0, 0, 0.3)
}

.dialog__card{
  background-color: #ffffff;
  padding: 24px;
  border-radius: 8px;
  text-align: center; /* block或inline 置中的方式 */
}

.dialog__input{
  display: block;
  font-size: 24px;
  padding: 8px;
  outline: 0; /* 預設效果歸0  */
  margin-bottom: 16px;
}

.dialog__confirm-btn{
  background-color: transparent;
  border: 2px solid #2196f3;
  border-radius: 20px;
  padding: 8px 16px; /* 上下 左右 */
  color: #2196f3;
  font-size: 18px;
  font-weight: 600;
  outline: 0;
  margin-right: 8px
}

.dialog__confirm-btn:active{
  transform: scale(0.95);
}

.dialog__cancle-btn{
  background-color: transparent;
  border: 2px solid #ff0000;
  border-radius: 20px;
  padding: 8px 16px; /* 上下 左右 */
  color: #ff0000;
  font-size: 18px;
  font-weight: 600;
  outline: 0;
  margin-left: 8px;
}

.dialog__cancle-btn:active{
  transform: scale(0.95);
}

.todo-list {
  max-width: 700px;
  padding: 16px;
  margin: auto;
}

.todo-list__item{
  padding: 16px;
  margin: 8px auto;
  height: 70px;
  border-radius: 8px;
  box-shadow: 0 0 4px rgba(0, 0, 0, 0.3); /* 4個參數: 1.陰影的水平位移 2.陰影的垂直位移 3.陰影的寬度 4.陰影的顏色 */
  display: flex;
  align-items: center;
  justify-content: space-between ; /* 剩餘空間等分間隔 */
}
/* hover 滑鼠移過去的事件 */
.todo-list__item:hover .todo-item__delete-btn,
.todo-list__item:hover .todo-item__checkbox + label {
  visibility: visible;
}

.todo-item__checkbox{
  display: none;

}

.todo-item__checkbox + label{
  font-size: 24px;
  color: #2196f3;
  visibility: hidden;
  
}

/* checked 選擇被checked的按鈕 */
/* ~表示同層級 */
.todo-item__checkbox:checked ~ .todo-item__text{
  text-decoration: line-through;
  
}

.todo-item__text{
  font-size: 24px;
  font-weight: 700;
  color: #666666;

}

.todo-item__delete-btn{
  visibility: hidden;
  background-color: transparent;
  border: 2px solid #ff0000;
  border-radius: 20px;
  padding: 8px 16px; /* 上下 左右 */
  color: #ff0000;
  font-size: 18px;
  font-weight: 600;
  outline: 0;
  margin-left: 8px;

}

.todo-item__delete-btn:active{
  transform: scale(0.95);
}


</style>
