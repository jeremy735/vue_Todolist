<template>
     <div class="todo-header">
        <input type="text" placeholder="請輸入你的任務名稱，按enter確認" v-model= "title" @keyup.enter="add"/>
    </div>
</template>

<script>
    import {nanoid} from 'nanoid'
    export default{
        name:'MyHeader',
        props:['addTodo'],// 接收父親函數
        data(){
            return{
                title:''
            }
        },
        methods:{ 
            add(){
                //矯正對象
                if(!this.title.trim()) return alert('輸入對象不為空')
                //將用戶的輸入包裝成一個todo對象
                const todoObj = {id:nanoid(),title:this.title,done:false}
                //通知app 組件去添加一個todo對象
                this.addTodo(todoObj)
                //清空輸入
                this.title = ''
            }
        },

}


</script>

<style scoped>
    .todo-header input {
		width: 560px;
		height: 28px;
		font-size: 14px;
		border: 1px solid #ccc;
		border-radius: 4px;
		padding: 4px 7px;
	}
	.todo-header input:focus {
		outline: none;
		border-color: rgba(82, 168, 236, 0.8);
		box-shadow: inset 0 1px 1px rgba(0, 0, 0, 0.075), 0 0 8px rgba(82, 168, 236, 0.6);
	}

</style>
