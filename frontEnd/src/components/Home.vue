<template>
	<el-row class="panel">
		<el-col :span="24" class="panel-top">
			<el-col :span="18">
        <!-- <template v-if="logo_type == '1'">
          <img :src="img" class="logo">
        </template> -->
        <template>
          <img src="../assets/images/whg_logo.png" class="logo" @click="changeCollapse" style="cursor:pointer;">
          <span class="p-l-18 logo_title">{{title}}</span>
        </template>
			</el-col>
			<!-- <el-col :span="16" class="ofv-hd">
				<div class="fl p-l-20 p-r-20 top-menu" :class="{'top-active': menu.selected}" 
        v-for="menu in topMenu" @click="switchTopMenu(menu)">{{menu.title}}</div>
			</el-col> -->
			<el-col :span="6" class="pos-rel">
				<el-dropdown @command="handleMenu" class="user-menu">
		      <span class="el-dropdown-link c-gra" style="cursor: default">
		        {{username}}&nbsp;&nbsp;<i class="fa fa-user" aria-hidden="true"></i>
		      </span>
		      <el-dropdown-menu slot="dropdown">
		        <el-dropdown-item command="changePwd">修改密码</el-dropdown-item>
		        <el-dropdown-item command="logout">退出</el-dropdown-item>
		      </el-dropdown-menu>
		    </el-dropdown>
			</el-col>
		</el-col>
		<el-col :span="24" class="panel-center">
			<!--<el-col :span="4">-->
			<aside class="w-180 ovf-hd" v-show="!showLeftMenu">
				<leftMenu :menuData="menuData" :menu="menu" ref="leftMenu" :isCollapse="collapse"></leftMenu>
			</aside>
			<section class="panel-c-c" :class="{'hide-leftMenu': hasChildMenu,'collapse-left': collapse}">
				<div class="grid-content bg-purple-light">
					<el-col :span="24">
						<transition name="fade" mode="out-in" appear>
							<router-view v-loading="showLoading"></router-view>
						</transition>
					</el-col>
				</div>
			</section>
		</el-col>

		<changePwd ref="changePwd"></changePwd>

	</el-row>
</template>

<script>
  import leftMenu from './Common/leftMenu.vue'
  import changePwd from './Account/changePwd.vue'
  import http from '../assets/js/http'

  export default {
    data() {
      return {
        username: '',
        topMenu: [],
        childMenu: [],
        menuData: [],
        hasChildMenu: false,
        menu: null,
        module: null,
        img: '',
        title: '',
        logo_type: null,
        collapse:false,
      }
    },
    methods: {
      logout() {
        this.$confirm('确认退出吗?', '提示', {
          confirmButtonText: '确定',
          cancelButtonText: '取消'
        }).then(() => {
          _g.openGlobalLoading()
          let data = {
            authkey: Lockr.get('authKey'),
            sessionId: Lockr.get('sessionId')
          }
          this.apiPost('admin/base/logout', data).then((res) => {
            _g.closeGlobalLoading()
            this.handelResponse(res, (data) => {
              Lockr.rm('menus')
              Lockr.rm('authKey')
              Lockr.rm('rememberKey')
              Lockr.rm('authList')
              Lockr.rm('userInfo')
              Lockr.rm('sessionId')
              Cookies.remove('rememberPwd')
              _g.toastMsg('success', '退出成功')
              setTimeout(() => {
                router.replace('/')
              }, 1500)
            })
          })
        }).catch(() => {

        })
      },
      switchTopMenu(item) {
        if (!item.child) {
          router.push(item.url)
        } else {
          router.push(item.child[0].child[0].url)
        }
      },
      handleMenu(val) {
        switch (val) {
          case 'logout':
            this.logout()
            break
          case 'changePwd':
            this.changePwd()
            break
        }
      },
      changePwd() {
        this.$refs.changePwd.open()
      },
      getTitleAndLogo() {
        this.apiPost('admin/base/getConfigs').then((res) => {
          this.handelResponse(res, (data) => {
            document.title = '文化馆管理后台'
            this.logo_type = data.LOGO_TYPE
            this.title = '文化馆管理后台'
            this.img = window.HOST + data.SYSTEM_LOGO
            // this.img = '../assets/images/whg_logo.png'
          })
        })
      },
      getUsername() {
        this.username = Lockr.get('userInfo').username
      },
      //菜单折叠
      changeCollapse(){
        this.collapse = this.collapse ? false : true;
        console.log(this.collapse);
      }
    },
    created() {
      this.getTitleAndLogo()
      let authKey = Lockr.get('authKey')
      let sessionId = Lockr.get('sessionId')
      if (!authKey || !sessionId) {
        _g.toastMsg('warning', '您尚未登录')
        setTimeout(() => {
          router.replace('/')
        }, 1500)
        return
      }
      this.getUsername()
      let menus = Lockr.get('menus')
      this.menu = this.$route.meta.menu
      this.module = this.$route.meta.module
      this.topMenu = menus
      _(menus).forEach((res) => {
        if (res.module == this.module) {
          this.menuData = res.child
          res.selected = true
        } else {
          res.selected = false
        }
      })
    },
    computed: {
      routerShow() {
        return store.state.routerShow
      },
      showLeftMenu() {
        this.hasChildMenu = store.state.showLeftMenu
        return store.state.showLeftMenu
      }
    },
    components: {
      leftMenu,
      changePwd
    },
    watch: {
      '$route' (to, from) {
        _(this.topMenu).forEach((res) => {
          if (res.module == to.meta.module) {
            res.selected = true
            if (!to.meta.hideLeft) {
              this.menu = to.meta.menu
              this.menuData = res.child
            }
          } else {
            res.selected = false
          }
        })
      }
    },
    mixins: [http]
  }
</script>


<style>
	.fade-enter-active,
	.fade-leave-active {
		transition: opacity .5s
	}
	
	.fade-enter,
	.fade-leave-active {
		opacity: 0
	}
	
	.panel {
		position: absolute;
		top: 0px;
		bottom: 0px;
		width: 100%;
	}
	
	.panel-top {
		height: 60px;
		line-height: 60px;
		background: #1F2D3D;
		color: #ffffff;
	}
	
	.panel-center {
		background: #324057;
		position: absolute;
		top: 60px;
		bottom: 0px;
		overflow: hidden;
	}
	
	.panel-c-c {
		background: #f1f2f7;
		position: absolute;
		right: 0px;
		top: 0px;
		bottom: 0px;
		left: 179px;
    transition:left 0.7s;
    -moz-transition:left 0.7s; /* Firefox 4 */
    -webkit-transition:left 0.7s; /* Safari and Chrome */
    -o-transition:left 0.7s; /* Opera */
		overflow-y: scroll;
		padding: 20px;
	}
	
	.logout {
		background: url(../assets/images/logout_36.png);
		background-size: contain;
		width: 20px;
		height: 20px;
		float: left;
	}
	
	.logo {
		width: 40px;
    height: 40px;
		float: left;
    margin: 10px 5px 0 5px;
	}

  .logo_title{
    font-size: 20px;
    font-weight: 700;
  }
	
	.tip-logout {
		float: right;
		margin-right: 20px;
		padding-top: 5px;
		cursor: pointer;
	}
	
	.admin {
		color: #c0ccda;
		text-align: center;
	}
	.hide-leftMenu {
		left: 0px;
	}
  .collapse-left{
    left: 64px;
    transition:left 0.6s;
    -moz-transition:left 0.6s; /* Firefox 4 */
    -webkit-transition:left 0.6s; /* Safari and Chrome */
    -o-transition:left 0.6s; /* Opera */
  }
</style>
