<template>
    <transition name="todo" appear>
      <li>
          <label>
          <input 
            type="checkbox" 
            :checked="todo.done" 
            @change="handleCheck(todo.id)"
          />
          <span v-show="!todo.isEdit">{{todo.title}}</span>
          <input 
            type="text" 
            v-show="todo.isEdit" 
            :value="todo.title"
            @blur="handleBlur(todo, $event)"
            ref="inputTitle"
          />
          </label>
          <button class="btn btn-danger" @click="handleDelete(todo.id)">删除</button>
          <button v-show="!todo.isEdit" class="btn btn-edit" @click="handleEdit(todo)">编辑</button>
      </li>
    </transition>
</template>

<script>
    export default {
        name: 'TodoItem',
        props:['todo'],
        methods: {
            // 是否勾选
            handleCheck(id) {
                // 通知App组件改变对应的done值
                // this.changeDone(id)
                this.$bus.$emit('changeDone', id)
            },
            // 点击删除
            handleDelete(id) {
                if(confirm('确认删除吗？')) {
                    // 通知App组件删除对应todo
                    // this.deleteTodo(id)
                    this.$bus.$emit('deleteTodo', id)
                }
            },
            // 点击编辑
            handleEdit(todo) {
              if(todo.hasOwnProperty('isEdit')) {
                todo.isEdit = true
              } else {
                this.$set(todo, 'isEdit', true)
              }
              this.$nextTick(function() {
                this.$refs.inputTitle.focus()
              })
            },
            // 失去焦点
            handleBlur(todo,e) {
              todo.isEdit = false
              if(!e.target.value.trim()) return alert('输入不能为空!')
              this.$bus.$emit('updateData', todo.id, e.target.value)
            }
        },
    }
</script>

<style scoped>
/*item*/
  li {
    list-style: none;
    height: 36px;
    line-height: 36px;
    padding: 0 5px;
    border-bottom: 1px solid #ddd;
  }

  li label {
    float: left;
    cursor: pointer;
  }

  li label li input {
    vertical-align: middle;
    margin-right: 6px;
    position: relative;
    top: -1px;
  }

  li button {
    float: right;
    display: none;
    margin-top: 3px;
  }

  li:before {
    content: initial;
  }

  li:last-child {
    border-bottom: none;
  }

  li:hover {
    background-color: #ddd;
  }
  li:hover button {
    display: block;
  }

  .todo-enter-active {
    animation: todoTransition 0.5s linear;
  }

  .todo-leave-active {
    animation: todoTransition 0.5s linear reverse;
  }

  @keyframes todoTransition {
    from {
      transform: translateX(100%);
    }
    to {
      transform: translateX(0);
    }
  }
</style>