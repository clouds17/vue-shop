<template>
    <div>
        <div class="login_container">
            <div class="login_box">
                <div class="avatar">
                    <img src="../assets/logo.png" alt="">
                </div>
                <el-form :model="loginForm" :rules="loginFormRules" ref="loginFormRef" >
                    <el-form-item prop="username">
                        <el-input prefix-icon="iconfont icon-user" v-model="loginForm.username"></el-input>
                    </el-form-item>
                    <el-form-item prop="password">
                        <el-input v-model="loginForm.password" type="password" prefix-icon="iconfont icon-3702mima"></el-input>
                    </el-form-item>
                    <el-form-item class="btns">
                        <el-button type="primary" @click="login">立刻创建</el-button>
                        <el-button @click="resetForm">重置</el-button>
                    </el-form-item>
                </el-form>
            </div>
        </div>
    </div>
</template>
<script>
// import func from '../../vue-temp/vue-editor-bridge'
export default {
  data () {
    return {
      loginForm: {
        username: 'admin',
        password: '123456'
      },
      loginFormRules: {
        username: [
          { required: true, message: '请输入用户名称', trigger: 'blur' },
          { min: 3, max: 10, message: '长度在 3 到 10 个字符', trigger: 'blur' }
        ],
        password: [
          { required: true, message: '请输入用户密码', trigger: 'blur' },
          { min: 6, max: 15, message: '长度在 6 到 15 个字符', trigger: 'blur' }
        ]
      }
    }
  },
  methods: {
    resetForm: function () {
      this.$refs.loginFormRef.resetFields()
    },
    login: function () {
      this.$refs.loginFormRef.validate(async valid => {
        if (!valid) return 0
        const { data: res } = await this.$http.post('/login', this.loginForm)
        if (res.meta.status !== 200) {
          return this.$message.error('登录失败')
        }
        this.$message.success('登录成功')
        window.sessionStorage.setItem('token', res.data.token)
        this.$router.push('/home')
      })
    }
  }
}
</script>

<style lang="less" scoped>
.login_container {
    background-color: #2b4b6b;
    height: 100%;
    overflow: hidden;
}
.login_box {
    width: 400px;
    height: 300px;
    background-color: #fff;
    border-radius: 10px;
    margin: 150px auto;
    padding: 0 20px;
    overflow: hidden;
}
.el-form {
    margin-top: 100px;
}
.avatar {
    width: 130px;
    height: 130px;
    position: absolute;
    left: 50%;
    transform: translate(-50%, -50%);
    background-color: #fff;
    border-radius: 50%;
    overflow: hidden;
    box-shadow: 0 0 5px #ccc;
    padding: 10px;
}
.avatar img {
    width: 100%;
    height: 100%;
    border-radius: 50%;
    background-color: #ccc;
    border: 1px solid #ccc;
}
.btns {
    display: flex;
    justify-content: flex-end;
}
</style>
