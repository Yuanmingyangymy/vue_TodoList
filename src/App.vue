<template>
  <div id="root">
    <div class="todo-container">
      <div class="todo-wrap">
        <TodoHeader @addTodo="addTodo"/>
        <TodoList :todos="todos"/>
        <TodoFooter 
          :todos="todos" 
          @checkAllTodo="checkAllTodo"  
          @clearAllTodo="clearAllTodo"
        />
      </div>
    </div>
  </div>
</template>

<script>
  import TodoHeader from './components/TodoHeader.vue'
  import TodoList from './components/TodoList.vue'
  import TodoFooter from './components/TodoFooter.vue'
  export default {
    name: 'App',
    components: {
      TodoHeader,
      TodoList,
      TodoFooter
    },
    data() {
        return {
            todos: JSON.parse(localStorage.getItem('todos')) || []
        }
    },
    methods: {
      // 添加数据
      addTodo(todo) {
        this.todos.unshift(todo)
      },
      // 改变done值
      changeDone(id) {
        this.todos.forEach(todo => {
          if(todo.id === id) {
            todo.done = !todo.done
          }
        })
      },
      // 删除一个todo
      deleteTodo(id) {
        this.todos = this.todos.filter(todo => todo.id !== id)
      },
      // 全选or全不选
      checkAllTodo(done) {
        this.todos.forEach(todo => todo.done = done)
      },
      // 清楚已完成的todo
      clearAllTodo() {
        this.todos = this.todos.filter(todo => todo.done === false)
      },
      // 更新数据
      updateData(id, value) {
        this.todos.forEach(todo => {
          if(todo.id === id) {
            todo.title = value
          }
        })
      }
    },
    watch: {
      todos:{
        deep: true,
        handler(value) {
          localStorage.setItem('todos', JSON.stringify(value))
        }
      }
    },
    mounted() {
      this.$bus.$on('changeDone', this.changeDone)
      this.$bus.$on('deleteTodo', this.deleteTodo)
      this.$bus.$on('updateData', this.updateData)
    },
    beforeDestroy() {
      this.$bus.$off('changeDone')
      this.$bus.$off('deleteTodo')
      this.$bus.$off('updateData')
    },
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
    border: 1px solid rgb(0, 132, 255);
    margin-right: 5px;
  }

  .btn-danger:hover {
    color: #fff;
    background-color: #bd362f;
  }

  .btn-edit:hover {
    color: #fff;
    background-color: rgb(0, 132, 255);
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