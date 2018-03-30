// todos.vue  // 左侧列表    交互型组件
<template>
  <div class="page lists-show"><!--最外层容器-->
    <nav><!--容器上半部分-->
      <div class="nav-group"> <!--移动端的菜单图标-->
        <a class="nav-item">
          <span class="icon-list-unordered">
          </span>
        </a>
      </div>
      <h1 class="title-page">
        <span class="title-wrapper">{{todo.title}}</span> <!-- 标题-->
        <span class="count-list">{{todo.count}}</span> <!-- 数目-->
      </h1>
      <div class="nav-group right"> <!-- 右边的删除，锁定图标容器-->
        <div class="options-web">
          <a class=" nav-item"> <!-- 锁定图标-->
            <span class="icon-lock" v-if="todo.locked"></span>
            <span class="icon-unlock" v-else>
            </span>
          </a>
          <a class=" nav-item"><!-- 删除图标-->
            <span class="icon-trash">
            </span>
          </a>
        </div>
      </div>
      <div class=" form todo-new input-symbol">  <!-- 新增单个代办单项输入框,监听了回车事件，双向绑定text值,监听了disabled属性，在todo.locked为true的情况下无法编辑-->
         <input type="text" v-model="text" placeholder='请输入' @keyup.enter="onAdd" :disabled="todo.locked" />
        <span class="icon-add"></span>
      </div>
    </nav>
    <div class="content-scrollable list-items">
      <!--容器下半部分-->
      <div v-for="item in items">
        <item :item="item"></item>
      </div>
    </div>
  </div>
</template>
<script>
import item from './item'
import { getTodo, addRecord } from '../api/api'
export default {
  components: {
    item
  },
  // data() {
  //   return {
  //     todo: {
  //       title: '星期一',
  //       count: 12,
  //       locked: false
  //     },
  //     items: [
  //       { checked: false, text: '新的一天', isDelete: false },
  //       { checked: false, text: '新的一天', isDelete: false },
  //       { checked: false, text: '新的一天', isDelete: false }
  //     ],
  //     text: ''
  //   }
  // },
  watch: {
    '$route.parmas.id'() {
      this.init()
    }
  },
  created() {
    this.init()
  },
  methods: {
    init() {
      const ID = this.$route.params.id
      getTodo({ id: ID }).then(res => {
        let { id, title, count, isDelete, locked, record } = res.data.todo
        this.items = record
        this.todo = {
          id: id,
          title: title,
          count: count,
          locked: locked,
          isDelete: isDelete
        }
      })
    },
    onAdd() {
      // 当用户输入文字，并且回车时调用次方法。
      const ID = this.$route.params.id
      addRecord({ id: ID, text: this.text }).then(res => {
        this.text = ''
        this.init()
        // 请求成功后初始化
      })
    }
  }
}
</script>

<style lang = "less">
@import '../common/style/nav.less';
@import '../common/style/form.less';
@import '../common/style/todo.less';
</style>
