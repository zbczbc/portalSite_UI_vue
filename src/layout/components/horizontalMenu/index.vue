<!--
 * @Author: your name
 * @Date: 2020-02-23 18:16:10
 * @LastEditTime: 2020-03-18 09:47:51
 * @LastEditors: Please set LastEditors
 * @Description: In User Settings Edit
 * @FilePath: \portalSite_UI_vue\src\layout\components\horizontalMenu\index.vue
 -->
<template>
    <div class="navbar" >
      <logo></logo>
      <!------------导航菜单---------------->
     
        <el-menu
        mode="horizontal"
        background-color="#fff"
        text-color="#000"
        active-text-color="#49aff3"
        :unique-opened="true"
        :default-active="activeMenu"
        router
      >
         <nav-menu v-for="route in permission_routes" :key="route.path" :item="route" :base-path="route.path" ></nav-menu>
        </el-menu>
    
        
    
    <div class="header-content" >
      <!--搜索框--->
      <el-input v-model="fastsearch"  class="header-search" placeholder="请输入" >
        <el-button slot="append" icon="el-icon-search" style="color: #fff" @click="searchEnter"></el-button>
         <!--  <template slot="append" >
              <i class="el-icon-search" style="color: #fff" @click="searchEnter"  ></i>
           </template> -->
        </el-input>
    <!-- 登录框--->
     <div class="header-login" v-show="!loginStatue" >
           <div class="pan-btn light-blue-btn" @click="showLogin">
             去登录
           </div>
     </div>
     <!--登录下拉菜单-->
     <div v-show="loginStatue" style="margin-left: 10px;">
      <el-dropdown @command="handleCommand" trigger="click">
        <span class="el-dropdown-link">
             {{user_avatar}}<i class="el-icon-arrow-down el-icon--right"></i>
        </span>
        <el-dropdown-menu slot="dropdown">
         <!--  <el-dropdown-item command="a">个人信息</el-dropdown-item> -->
          <el-dropdown-item command="b">退出</el-dropdown-item>
        </el-dropdown-menu>
      </el-dropdown>
     </div>
    </div>
        
     <login ref="login"></login>
    </div>
</template>

<script>
import path from 'path'
import {mapGetters} from 'vuex'
import NavMenu from './NavMenu'
import Item from './Item'
import Logo from './Logo'
import Login from '@/views/login/login.vue'

export default {
    data(){
        return{
          onlyOneChild:[],
          searchVlaue:null,
          fastsearch:null,
        }
    },
    components:{
        Item,
        NavMenu,
        Logo,
        Login
    },
    computed: {
      ...mapGetters([
         'permission_routes',
         'user_name',
         'user_avatar'
       ]),
        activeMenu(){
          const route = this.$route
          const { meta,path } = route
          if(meta.activeMenu){
            return meta.activeMenu
          }
          return path
        },
        loginStatue(){
       
          if(this.$store.getters.login_status){
            return this.$store.getters.login_status
          }
          else{
            return false
          }
        }
    },
    methods:{
      // 显示登录框
      showLogin(){
        this.$refs['login'].handleShowDialog()
      },
      //跳转
      searchEnter(){
        if(this.fastsearch === null || this.fastsearch === ""){
          this.$message.info("请输入搜索内容")
          return ;
        }else{
          this.$router.push({path:'/search',query:{fastsearch:this.fastsearch}})
        }
        
      },
      //用户下拉菜单指令
      handleCommand(value){
        switch(value){
          case 'a':{
            
          };break;
          case 'b':{
            this.$store.dispatch('user/logout').then(()=>{
              this.$message({
                message:'退出成功',
                type:'success'
              })
            })
          }
        }
      }
    },
    mounted(){
    }
}

</script>
