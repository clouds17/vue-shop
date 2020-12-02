<template>
    <el-container>
        <el-header>
            <div>
                <img src="../assets/heima.png" alt="">
                <span>电商后台管理系统</span>
            </div>
            <el-button type="info" @click="logout">退出</el-button>
        </el-header>
        <el-container>
            <el-aside :width="iscollapse ? '65px' : '200px'">
                <div class="toggle-button" @click="button_collapse">|||</div>
                <el-menu
                background-color="#303440"
                text-color="#fff"
                active-text-color="#ffd04b"
                router
                :collapse= "iscollapse"
                :collapse-transition= "false"
                :default-active= "activePath">
                    <el-submenu :index="item.id+''" v-for="item in menuList" :key="item.id">
                        <template slot="title">
                        <i :class="menuIcon[item.order]"></i>
                        <span class="ml-10">{{item.authName}}</span>
                        </template>
                        <el-menu-item :index="'/'+children.path" v-for="children in item.children" :key="children.id"
                         @click="activeAdress('/'+children.path)">
                            <i class="el-icon-menu"></i>
                            <span slot="title">{{children.authName}}</span>
                        </el-menu-item>
                    </el-submenu>
                </el-menu>
            </el-aside>
            <el-main><router-view></router-view></el-main>
        </el-container>
    </el-container>
</template>
<script>
export default {
  data() {
    return {
      menuList: [],
      menuIcon: {
        1: 'iconfont icon-users',
        2: 'iconfont icon-tijikongjian',
        3: 'iconfont icon-shangpin',
        4: 'iconfont icon-danju',
        5: 'iconfont icon-baobiao'
      },
      iscollapse: false,
      activePath: ''
    }
  },
  created() {
    this.getMenuList()
  },
  methods: {
    async getMenuList() {
      const { data: res } = await this.$http.get('menus')
      if (res.meta.status !== 200) return this.$message.error('获取菜单列表失败')
      this.menuList = res.data
      console.log(this.menuList)
    },
    activeAdress(path) {
      window.sessionStorage.setItem('active', path)
      this.activePath = path
    },
    button_collapse() {
      this.iscollapse = !this.iscollapse
    },
    logout() {
      window.sessionStorage.clear()
      this.$router.push('/login')
    }
  }
}
</script>

<style lang="less" scoped>
.el-container,
.el-aside,
.el-main {
    height: 100%;
}
.el-aside {
    background-color: #303440;
}
.el-menu {
    border: 0;
}
.el-header {
    height: 60px;
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding-left: 0;
    background-color: #34393A;
    img {
        vertical-align: middle;
    }
    span {
        line-height: 60px;
        font-size: 16px;
        margin-left: 15px;
        color: #fff;
    }
    .el-button {
        margin-right: 20px;
    }
}
.toggle-button {
    height: 20px;
    background-color: #4A5060;
    color: #fff;
    font-size: 10px;
    letter-spacing: 5px;
    text-align: center;
    line-height: 20px;
    cursor: pointer;
}
</style>
