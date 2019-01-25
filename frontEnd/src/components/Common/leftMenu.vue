<template>
<!-- <div>
<el-menu mode="vertical" default-active="/table" class="el-menu-vertical-demo" @select="handleselect" theme="dark" router>
<el-menu-item-group v-for="menu in menuData" :title="menu.title">
<el-menu-item v-for="item in menu.items" :index="item.path">&nbsp;&nbsp;&nbsp;&nbsp;{{item.name}}</el-menu-item>
</el-menu-item-group>
</el-menu>
</div> -->

	<div class="left-nav-menu-wrap left_nav_box">  
    <el-menu class="sidebar-el-menu" :collapse="isCollapse" background-color="#324057"
             text-color="#bfcbd9" active-text-color="#20a0ff" :unique-opened="true">
      <template v-for="secMenu in menuData">
        <template v-if="secMenu.child">
          <el-submenu :index="secMenu.title">
            <template slot="title">
              <i :class="secMenu.icon"></i>
              <span>{{secMenu.title}}</span>
            </template>
            <template v-for="item in secMenu.child">
              <el-menu-item-group>
                <el-menu-item :index="item.title" :key="item.title" @click="routerChange(item)">
                  {{ item.title }}
                </el-menu-item>
              </el-menu-item-group>
            </template>
          </el-submenu>  
        </template>
        <template v-else>
          <el-menu-item :index="secMenu.title" :key="secMenu.title">
            <i :class="secMenu.icon"></i>
            <span slot="title">{{ secMenu.title }}</span>
          </el-menu-item>
        </template>
      </template>    
    </el-menu>
	</div>
</template>

<script>
export default {
  props: ['menuData', 'menu','isCollapse'],
  data() {
    return {
      
    }
  },
  methods: {
    routerChange(item) 	{
      // 与当前页面路由相等则刷新页面
      if (item.url != this.$route.path) {
        router.push(item.url)
      } else {
        _g.shallowRefresh(this.$route.name)
      }
    }
  }
}
</script>

<style scoped>
  .el-menu{
    border: 0px;
  }
</style>
