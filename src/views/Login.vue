<template>
    <div class="login-wrap">
        <div class="ms-login">
            <div class="ms-title">实验室管理系统</div>
            <el-form :model="param" :rules="rules" ref="login" label-width="0px" class="ms-content">
                <el-form-item prop="username">
                    <el-input v-model="param.username" placeholder="username">
                        <template #prepend>
                            <el-button icon="el-icon-user"></el-button>
                        </template>
                    </el-input>
                </el-form-item>
                <el-form-item prop="password">
                    <el-input
                        type="password"
                        placeholder="password"
                        v-model="param.password"
                        @keyup.enter="submitForm()"
                    >
                        <template #prepend>
                            <el-button icon="el-icon-lock"></el-button>
                        </template>
                    </el-input>
                </el-form-item>
                <div class="login-btn">
                    <el-button type="primary" @click="submitForm()">登录</el-button>
                </div>
                <p class="login-tips">Tips : 忘记密码请联系辅导员找回。</p>
            </el-form>
        </div>
    </div>
</template>

<script>
import {loginValidate} from "@/api/account";

export default {
    data() {
        return {
            param: {
                username: "admin",
                password: "123123",
            },
            accountInfo: {
              userId: "",
              username: "admin",
              name: "黎俊杰",
              studentName: "17320110",
              identity: "3",
            },
            rules: {
                username: [
                    { required: true, message: "请输入用户名", trigger: "blur" }
                ],
                password: [
                    { required: true, message: "请输入密码", trigger: "blur" }
                ]
            }
        };
    },
    created() {
        this.$store.commit("clearTags");
    },
    methods: {
        login(){
          loginValidate(this.param).then(response => {
            if (response.success){
              this.accountInfo = response.data
            }
          })
        },
        submitForm() {
            this.$refs.login.validate(valid => {
                if (valid) {
                    loginValidate(this.param).then(response => {
                      if (response.success){
                        this.accountInfo = response.data
                        localStorage.setItem("ms_username", this.accountInfo.username);
                        //这里设置用户名, 学号,身份等信息(用于表单提交时提供信息)
                        localStorage.setItem("ms_name", this.accountInfo.name);  //姓名
                        localStorage.setItem("ms_studentName", this.accountInfo.studentName);  //学号
                        localStorage.setItem("ms_identity", this.accountInfo.identity);
                        this.$message.success("登录成功");
                        this.$router.push("/");
                      }else{
                        this.$message.error("账号或密码错误");
                      }
                    })
                } else {
                    this.$message.error("请输入账号和密码");
                    return false;
                }
            });
        }
    }
};
</script>

<style scoped>
.login-wrap {
    position: relative;
    width: 100%;
    height: 100%;
    background: #324157;
    /*background-image: url(../assets/img/login-bg.jpg);*/
    background-size: 100%;
}
.ms-title {
    width: 100%;
    line-height: 50px;
    text-align: center;
    font-size: 20px;
    color: #fff;
    border-bottom: 1px solid #ddd;
}
.ms-login {
    position: absolute;
    left: 50%;
    top: 50%;
    width: 350px;
    margin: -190px 0 0 -175px;
    border-radius: 5px;
    background: rgba(255, 255, 255, 0.3);
    overflow: hidden;
}
.ms-content {
    padding: 30px 30px;
}
.login-btn {
    text-align: center;
}
.login-btn button {
    width: 100%;
    height: 36px;
    margin-bottom: 10px;
}
.login-tips {
    font-size: 12px;
    line-height: 30px;
    color: #fff;
}
</style>