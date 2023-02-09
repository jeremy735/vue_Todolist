<template>
  <div id="root">
		<div class="todo-container">
			<div class="todo-wrap">
				<MyHeader @addTodo="addTodo"/>
				<MyList :todos="todos" />
				<MyFooter :todos="todos" @checkAllTodo="checkAllTodo" @clearAllTodo="clearAllTodo"/>
			</div>
		</div>
	</div>
</template>

<script>
import MyHeader from './components/MyHeader.vue'
import MyList from './components/MyList.vue'
import MyFooter from './components/MyFooter.vue'


export default {
  name: 'App',
  components:{MyHeader,MyList,MyFooter},
  data(){
        return{
          // 先用json.parse解析後獨取值才不會一開始拿到json字符串
          todos:JSON.parse(localStorage.getItem('todos')) || [] // 注意: 一般來說用戶一開始不會有 todos 的資料在 LocalStorage，所以必須要利用 '或' 操作 如果 localStorage.getItem 返回值是一個 null ，就為 一個數組。
        }
    },
    methods:{
      //添加一個todo
      addTodo(todoObj){
        this.todos.unshift(todoObj)
      },
      //勾選或取消勾選一個todo
      checkTodo(id){
        this.todos.forEach((todo)=>{
          if(todo.id === id) todo.done =!todo.done
        })
      },
      updateTodo(id,title){
        this.todos.forEach((todo)=>{
          if(todo.id === id) todo.title = title
        })
      },
      deleteTodo(id){
       this.todos = this.todos.filter(todo => todo.id !== id )
      },
      checkAllTodo(done){
        this.todos.forEach((todo)=>{
          todo.done = done
        })
      },
      clearAllTodo(){
        this.todos = this.todos.filter((item)=>item.done !== true)
      }
    },
    watch:{
      todos:{
        deep:true,//加入深入監測保證勾選done發生變化時也要改變localStorage 的資料
        handler(value){
          //存入直是obj 先把數組轉為json 字符串後存入
          localStorage.setItem('todos',JSON.stringify(value))
        }
      }
    },
    mounted(){
      this.$bus.$on('checkTodo',this.checkTodo)
      this.$bus.$on('deleteTodo',this.deleteTodo)
      this.$bus.$on('updateTodo',this.updateTodo)
    },
    beforeDestroy(){
      this.$bus.$off('checkTodo')
      this.$bus.$off('deleteTodo')
      this.$bus.$off('updateTodo')
    }
  }

</script>

<style>
/*base*/
  body {
      background: #fff;
    }
    .btn {
      display: inline-block;
      padding: 4px 12px;
      margin-bottom: 0;
      font-size: 14px;
      line-height: 20px;
      text-align: center;
      vertical-align: middle;
      cursor: pointer;
      box-shadow: inset 0 1px 0 rgba(255, 255, 255, 0.2), 0 1px 2px rgba(0, 0, 0, 0.05);
      border-radius: 4px;
    }
    .btn-danger {
      color: #fff;
      background-color: #da4f49;
      border: 1px solid #bd362f;
    }
    .btn-edit {
      color: #fff;
      background-color: skyblue;
      border: 1px solid rgb(108, 188, 219);
      margin-right: 5px;
    }
    .btn-danger:hover {
      color: #fff;
      background-color: #bd362f;
    }
    .btn:focus {
      outline: none;
    }
    .todo-container {
      width: 600px;
      margin: 0 auto;
    }
    .todo-container .todo-wrap {
      padding: 10px;
      border: 1px solid #ddd;
      border-radius: 5px;
    }
</style>
