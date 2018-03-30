// layouts.vue  // 布局组件  接入型 接入其他组件

<template>
    <!-- 最外面的容器 -->
    <section class="container">
        <!-- 左边的容器 -->
        <section class="menu">
           <menus></menus>
        </section>
        <!-- 右边的容器 -->
        <section class="content-container">

        </section>
    </section>
</template>
<script>
import menus from './menus'
import todo from './todos'
export default {
  components: {
    menus,
    todo
  }
}
</script>
<style lang="less">
@import '../common/style/layouts';
</style>
