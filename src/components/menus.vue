<template>
  <div class="list-todos">  <!--菜单容器-->
      <a @click="goList(item.id)" class="list-todo activeListClass list" :class="{'active': item.id === todoId}"  v-for="item in items" :key="item.id"> <!--单个菜单容器-->
        <span class="icon-lock" v-if="item.locked"></span>  <!--锁的图标-->
        <span class="count-list" v-if="item.count">0">{{item.count}}</span><!--数字-->
        {{item.title}}<!--菜单内容-->
      </a>
    <a class=" link-list-new" > <!--新增菜单-->
      <span class="icon-plus">
      </span>
      新增
    </a>
  </div>
</template>
<script>
import { getTodoList, addTodo } from '../api/api'
export default {
  data() {
    return {
      items: [],
      todoId: ''
      // items: [
      //   { id: 1, title: '星期一', count: 1, locked: true }, // 菜单的模拟数据
      //   { id: 2, title: '星期二', count: 2, locked: true },
      //   {
      //     id: 3,
      //     title: '星期三',
      //     count: 3,
      //     locked: false
      //   }
      // ]
    }
  },
  created() {
    getTodoList({}).then(res => {
      const TODOS = res.data.todos // 数据返回到res.data里面
      this.items = TODOS
      this.todoId = TODOS[0].id
    })
  },
  methods: {
    goList(id) {
      // 点击菜单时候,替换选中id
      this.todoId = id
    },
    addTodoList() {
      // 点击新增按钮时候
      // 调用新增菜单的接口，在接口调用成功在请求数据
      addTodo({}).then(data => {
        getTodoList({}).then(res => {
          const TODOS = res.data.todos
          this.todoId = TODOS[TODOS.length - 1].id
          this.items = TODOS
        })
      })
    }
  }
}
</script>
<style lang="less">
@import '../common/style/menu.less';
</style>
