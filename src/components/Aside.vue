<template>
  <!-- 导航栏 -->
  <el-aside width="200px" style="background-color: rgb(238, 241, 246)">
    <el-menu>
      <el-submenu index="1">
        <template slot="title"><i class="el-icon-message"></i>画作</template>
        <el-menu-item-group router="true">
          <template slot="title">类别</template>
          <el-menu-item index="1-1" @click="showAll">全部</el-menu-item>
          <el-menu-item index="1-2">国风</el-menu-item>
          <el-menu-item index="1-3">油画</el-menu-item>
          <el-menu-item index="1-4">漫画</el-menu-item>
          <el-menu-item index="1-5">水彩画</el-menu-item>
          <el-menu-item index="1-6">风景画</el-menu-item>

          <!-- 油画、国画、漫画 -->
        </el-menu-item-group>


      </el-submenu>
      <el-submenu index="2">
        <template slot="title"><i class="el-icon-menu"></i>导航二</template>
        <el-menu-item-group>
          <template slot="title">分组一</template>
          <el-menu-item index="2-1">选项1</el-menu-item>
          <el-menu-item index="2-2">选项2</el-menu-item>
        </el-menu-item-group>
        <el-menu-item-group title="分组2">
          <el-menu-item index="2-3">选项3</el-menu-item>
        </el-menu-item-group>
        <el-submenu index="2-4">
          <template slot="title">选项4</template>
          <el-menu-item index="2-4-1">选项4-1</el-menu-item>
        </el-submenu>
      </el-submenu>
      <el-submenu index="3">
        <template slot="title"><i class="el-icon-setting"></i>导航三</template>
        <el-menu-item-group>
          <template slot="title">分组一</template>
          <el-menu-item index="3-1">选项1</el-menu-item>
          <el-menu-item index="3-2">选项2</el-menu-item>
        </el-menu-item-group>
        <el-menu-item-group title="分组2">
          <el-menu-item index="3-3">选项3</el-menu-item>
        </el-menu-item-group>
        <el-submenu index="3-4">
          <template slot="title">选项4</template>
          <el-menu-item index="3-4-1">选项4-1</el-menu-item>
        </el-submenu>
      </el-submenu>
    </el-menu>
  </el-aside>
</template>

<script>
import VueRouter from 'vue-router'
export default {

  name: 'Aside',
  methods: {
    showAll() {
      // console.log(this.$router)

      this.$router.push({
        path: '/allart'
      })
    }
  }

}
</script>

</style>
