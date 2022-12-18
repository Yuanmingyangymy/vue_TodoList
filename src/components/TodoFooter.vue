<template>
    <div class="todo-footer" v-show="total">
        <label>
          <input type="checkbox" :checked="isAll" @click="checkAll"/>
        </label>
        <span>
          <span>已完成{{doneTodo}}</span> / 全部{{total}}
        </span>
        <button class="btn btn-danger" @click="clearAll">清除已完成任务</button>
    </div>
</template>

<script>
    export default {
        name: 'TodoFooter',
        // props:['todos', 'checkAllTodo', 'clearAllTodo'],
        props:['todos'],
        computed: {
          total() {
            return this.todos.length
          },
          doneTodo() {
            return this.todos.reduce((pre, todo) => pre + (todo.done ? 1 : 0), 0)
          },
          isAll() {
            return this.doneTodo === this.total && this.total > 0
          }
        },
        methods: {
          checkAll(e) {
            // 通知App组件对所有todo的done值取反
            // this.checkAllTodo(e.target.checked)
            this.$emit('checkAllTodo', e.target.checked)
          },
          clearAll() {
            if(confirm('确定清除已完成任务？')) {
              // 通知App组件实现清楚已完成的todo
              // this.clearAllTodo()
              this.$emit('clearAllTodo')
            }
          }
        }
    }
</script>

<style scoped>
/*footer*/
  .todo-footer {
    height: 40px;
    line-height: 40px;
    padding-left: 6px;
    margin-top: 5px;
  }

  .todo-footer label {
    display: inline-block;
    margin-right: 20px;
    cursor: pointer;
  }

  .todo-footer label input {
    position: relative;
    top: -1px;
    vertical-align: middle;
    margin-right: 5px;
  }

  .todo-footer button {
    float: right;
    margin-top: 5px;
  }
</style>