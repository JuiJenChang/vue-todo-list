<template>
  <div class="todo-list-page">
    <h1>Todo List</h1>
    <div class="todo-input">
      <input type="text" v-model="todo" @keyup.enter="addTodo" placeholder="增新事項" :disabled="isInput"/>
      <button @click="addTodo" :disabled="isInput">新增</button>
    </div>

    <span v-if="time !== null" class="timer">{{ time }} 秒後才能再新增</span>

    <el-tabs v-model="visibility" type="border-card">
      <el-tab-pane name="all" label="全部">
        <ul>
          <li v-for="item in todoList" :key="item.id">
            <div v-if="item.id !== cacheTodo.id" class="content">
              <input type="checkbox" v-model="item.completed"/>
              <label :for="item.id" :class="{'completed': item.completed}">
                {{ item.title }}
              </label>
              <div class="operating">
                <i class="el-icon-edit" @click="editTodo(item)"></i>
                <i class="el-icon-close" @click="removeTodo(item.id)"></i>
              </div>
            </div>
            <div class="editInput">
              <input
                type="text"
                v-model="cacheTitle"
                v-if="item.id === cacheTodo.id"
                @keyup.esc="cacheTodo = {}"
                @keyup.enter="doneEdit(item)"
              />
            </div>
          </li>
        </ul>
      </el-tab-pane>
      <el-tab-pane name="active" label="進行中">
        <ul>
          <li v-for="item in filteredTodoList" :key="item.id">
            <div v-if="item.id !== cacheTodo.id" class="content">
              <input type="checkbox" v-model="item.completed"/>
              <label :for="item.id" :class="{'completed': item.completed}">
                {{ item.title }}
              </label>
              <div class="operating">
                <i class="el-icon-edit" @click="editTodo(item)"></i>
                <i class="el-icon-close" @click="removeTodo(item.id)"></i>
              </div>
            </div>
            <div class="editInput">
              <input
                type="text"
                v-model="cacheTitle"
                v-if="item.id === cacheTodo.id"
                @keyup.esc="cacheTodo = {}"
                @keyup.enter="doneEdit(item)"
              />
            </div>
          </li>
        </ul>
      </el-tab-pane>
      <el-tab-pane name="completed" label="已完成">
        <ul>
          <li v-for="item in filteredTodoList" :key="item.id">
            <div v-if="item.id !== cacheTodo.id" class="content">
              <input type="checkbox" v-model="item.completed"/>
              <label :for="item.id" :class="{'completed': item.completed}">
                {{ item.title }}
              </label>
              <div class="operating">
                <i class="el-icon-close" @click="removeTodo(item.id)"></i>
              </div>
            </div>
          </li>
        </ul>
      </el-tab-pane>
    </el-tabs>
  </div>
</template>

<script>
  export default {
    name: 'TodoList',
    data() {
      return {
        todo: '',
        todoList: [],
        visibility: 'all',
        cacheTodo: {},
        cacheTitle: '',
        timer: null,
        time: null,
        isInput: false
      }
    },
    computed: {
      filteredTodoList() {
        if (this.visibility === 'active') {
          let newList = []
          this.todoList.forEach(item => {
            if (!item.completed) {
              newList.push(item)
            }
          })
          return newList
        } else if (this.visibility === 'completed') {
          let newList = []
          this.todoList.forEach(item => {
            if (item.completed) {
              newList.push(item)
            }
          })
          return newList
        }
        return []
      }
    },
    methods: {
      countDown() {
        if (this.time !== 0) {
          this.time --
        } else {
          this.isInput = false
          this.time = null
          clearInterval(this.timer)
        }
      },
      addTodo() {
        if (!this.todo) {
          return
        }
        const timestamp = Math.floor(Date.now())
        this.todoList.push({
          id: timestamp,
          title: this.todo.trim(),
          completed: false
        })
        this.time = 2
        this.isInput = true
        this.todo = ''
        this.timer = setInterval(this.countDown, 1000)
      },
      editTodo(item) {
        this.cacheTodo = item
        this.cacheTitle = item.title
      },
      doneEdit(item) {
        item.title = this.cacheTitle
        this.cacheTitle = ''
        this.cacheTodo = {}
      },
      removeTodo(id) {
        this.todoList = this.todoList.filter(item => item.id !== id)
      }
    },
    beforeDestroy() {
      clearInterval(this.timer)
    }
  }
</script>

<style lang="scss" scoped>
.todo-list-page {
  display: flex;
  flex-direction: column;
  align-items: center;
  background: #bafaff;
  height: 100%;
  h1 {
    margin-bottom: 20px;
    padding-top: 50px;
    font-size: 32px;
    font-weight: 600;
    color: #388eff;
  }
}
.todo-input {
  width: 30%;
  border: 1px solid #388eff;
  display: flex;
  input {
    border: none;
    outline: none;
    width: 100%;
  }
  button {
    border: none;
    outline: none;
    width: 30%;
    cursor: pointer;
    background: #388eff;
    color: #fff;
  }
}
.timer {
  color: #ff6a6a;
  font-size: 12px;
}
.el-tabs {
  width: 70%;
  margin-top: 20px;
  li {
    margin-bottom: 20px;
    .content {
      display: flex;
      justify-content: space-between;
    }
    .editInput {
      display: flex;
      justify-content: center;
      input {
        outline: none;
        border: none;
        border-bottom: 1px solid #388eff;
        width: 30%;
      }
    }
  }
  li:last-child {
    margin-bottom: 0;
  }
}
.completed {
  text-decoration: line-through;
}
.operating {
  width: 50px;
  display: flex;
  justify-content: space-between;
  i {
    cursor: pointer;
  }
}
</style>