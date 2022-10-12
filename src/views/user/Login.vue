<template>
  <div class="login_container" v-loading="loading">
    <div class="bg-squares">
      <li></li>
      <li></li>
      <li></li>
      <li></li>
      <li></li>
      <li></li>
      <li></li>
      <li></li>
      <li></li>
      <li></li>
      <li></li>
      <li></li>
      <li></li>
      <li></li>
      <li></li>
    </div>
    <h1 class="welcome_text">欢迎进入体育场地申请系统</h1>
    <div class="login_box">
      <!-- 头像区域 -->
      <div class="avatat_box">
        <img src="../../assets/imgs/photo.jpg" alt="" />
      </div>
      <!-- 登录表单区域 -->
      <el-form ref="loginFormRef" class="login_form" :model="LoginForm" :rules="LoginFormRules" label-width="10%">
        <!-- 用户名 -->
        <el-form-item prop="user_name">
          <el-input placeholder="请输入用户名" prefix-icon="el-icon-user" v-model="LoginForm.user_name"></el-input>
        </el-form-item>
        <!-- 密码 -->
        <el-form-item prop="password">
          <el-input placeholder="请输入用户密码" prefix-icon="el-icon-lock" v-model="LoginForm.password" type="password">
          </el-input>
        </el-form-item>
      </el-form>
      <!-- 按钮区 -->
      <div class="LoginBtns">
        <el-button type="primary" round @click="adminLogin">管理员登录</el-button>
        <el-button type="success" @click="dialogFormVisible = true" round>注册</el-button>
        <el-button type="primary" round @click="login">登录</el-button>
      </div>
      
    </div>

    <!-- 注册界面 -->
    <!-- Form -->
    <el-dialog :visible="dialogFormVisible" title="注册" width="80%" :show-close="false">
      <el-form :model="registerForm" status-icon ref="registerFormRef" :rules="registerFormRules">
        <el-row>
          <el-col :span="10">
            <el-form-item label="学号:" :label-width="formLabelWidth" prop="sno">
              <el-input v-model="registerForm.sno" size="mini" prefix-icon="el-icon-edit">
              </el-input>
            </el-form-item>
          </el-col>
          <el-col :span="10">
            <el-form-item label="密码:" :label-width="formLabelWidth" prop="password">
              <el-input v-model="registerForm.password" type="password" size="mini" prefix-icon="el-icon-lock">
              </el-input>
            </el-form-item>
          </el-col>
        </el-row>

        <el-row>
          <el-col :span="10">
            <el-form-item label="姓名:" :label-width="formLabelWidth" prop="user_name">
              <el-input v-model="registerForm.user_name" size="mini" prefix-icon="el-icon-user">
              </el-input>
            </el-form-item>
          </el-col>
          <el-col :span="10">
            <el-form-item label="班级:" :label-width="formLabelWidth" prop="user_class">
              <el-input v-model="registerForm.user_class" size="mini" prefix-icon="el-icon-edit">
              </el-input>
            </el-form-item>
          </el-col>
        </el-row>

        <el-row>
          <el-col :span="10">
            <el-form-item label="学院:" :label-width="formLabelWidth" prop="faculty">
              <el-input v-model="registerForm.faculty" size="mini" prefix-icon="el-icon-edit">
              </el-input>
            </el-form-item>
          </el-col>
          <el-col :span="10">
            <el-form-item label="专业:" :label-width="formLabelWidth" prop="profession">
              <el-input v-model="registerForm.profession" size="mini" prefix-icon="el-icon-edit">
              </el-input>
            </el-form-item>
          </el-col>
        </el-row>

        <el-row>
          <el-col :span="10">
            <el-form-item label="联系方式:" :label-width="formLabelWidth" prop="telephone">
              <el-input v-model="registerForm.telephone" size="mini" prefix-icon="el-icon-phone">
              </el-input>
            </el-form-item>
          </el-col>
          <el-col :span="10">
            <el-form-item label="性别:" :label-width="formLabelWidth" prop="gender">
              <el-radio-group v-model="registerForm.gender" style="margin-top: 8px">
                <el-radio label="男">男</el-radio>
                <el-radio label="女">女</el-radio>
              </el-radio-group>
            </el-form-item>
          </el-col>
        </el-row>
        <el-row>
          <el-col :span="10">
            <el-form-item label="邮箱:" :label-width="formLabelWidth" prop="mail">
              <el-input v-model="registerForm.mail" size="mini" prefix-icon="el-icon-message">
              </el-input>
            </el-form-item>
          </el-col>
        </el-row>
      </el-form>
      <div slot="footer" class="dialog-footer">
        <el-button @click="dialogFormVisible = false">取 消</el-button>
        <el-button type="primary" @click="doRegister">确 定</el-button>
      </div>

    </el-dialog>
    <!--  -->
  </div>
</template>

<script>
import { login_, register_ } from "../../network/userApi";
export default {
  name: "Login",
  components: {},
  data() {
    return {
      loading: false,
      dialogVisible: false,
      LoginForm: {
        user_name: "",
        password: "",
      },
      registerForm: {
        sno: "",
        user_name: "",
        gender: "",
        user_class: "",
        faculty: "",
        profession: "",
        telephone: "",
        password: "",
        mail: "",
      },
      LoginFormRules: {
        /* 验证用户名是否合法 */
        user_name: [
          {
            required: true,
            message: "请输入登录名",
            trigger: "blur",
          },
          {
            min: 2,
            max: 20,
            message: "长度需要2-20个字符",
            trigger: "blur",
          },
        ],
        /* 验证密码是否合法 */
        password: [
          {
            required: true,
            message: "请输入登录密码",
          },
          {
            min: 6,
            max: 15,
            message: "长度需要6-15个字符",
            trigger: "blur",
          },
        ],
      },
      registerFormRules: {
        sno: [
          {
            required: true,
            message: "请输入学号",
            trigger: "blur",
          },
        ],
        user_name: [
          {
            required: true,
            message: "请输入姓名",
            trigger: "blur",
          },
        ],
        gender: [
          {
            required: true,
            message: "请选择性别",
            trigger: "blur",
          },
        ],
        user_class: [
          {
            required: true,
            message: "请输入班级",
            trigger: "blur",
          },
        ],
        faculty: [
          {
            required: true,
            message: "请输入学院",
            trigger: "blur",
          },
        ],
        profession: [
          {
            required: true,
            message: "请输入专业",
            trigger: "blur",
          },
        ],
        telephone: [
          {
            required: true,
            message: "请输入电话号码",
            trigger: "blur",
          },
        ],
        password: [
          {
            required: true,
            message: "请输入注册密码",
          },
          {
            min: 6,
            max: 15,
            message: "长度需要6-15个字符",
            trigger: "blur",
          },
        ],
        mail: [
          {
            required: false,
            message: "请输入邮箱",
            trigger: "blur",
          },
        ],
      },
      dialogFormVisible: false,
      formLabelWidth: "30%",
    };
  },
  methods: {
    /* 点击登录按钮，登录 */
    login() {
      this.loading = true,
        this.$refs.loginFormRef.validate(async (valid) => {
          if (valid) {
            login_(this.LoginForm).then(
              (res) => {
                if (res.status == 0) {
                  window.sessionStorage.setItem("token", res.data.token);
                  this.$message.success(`登录成功，欢迎进入`);
                  this.$router.replace("/home");
                }
              },
              (err) => {
                return this.$message.error(err.message);
              }
            );
            this.loading = false;
          }
          if (!valid) {
            this.loading = false;
            return this.$message.error("登录失败");
          }
        });
    },
    /* 点击注册按钮 */
    doRegister() {
      this.$refs.registerFormRef.validate(async (valid) => {
        if (valid) {
          register_(this.registerForm).then(
            (res) => {
              if (res.status == 0) {
                this.$message.success(res.message);
                window.sessionStorage.setItem("token", res.data.token);
                this.$router.replace("/home");
              } else {
                console.log(res);
              }
            },
            (err) => {
              console.log(err);
            }
          );
        }
      });
    },
    adminLogin() {
      window.sessionStorage.setItem("token", "token");
      this.$message.success(`您已管理员身份进入！`);
      this.$router.push("/adminHome");
    },
  },
};
</script>

<style scoped>
/* 背景方块 */
.bg-squares {
  list-style: none;
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  z-index: 1;
}

.bg-squares li {
  width: 40px;
  height: 40px;
  background-color: rgba(255, 255, 255, .15);
  position: absolute;
  bottom: -10px;
  animation: square 20s linear infinite;
}

.bg-squares li:nth-child(1) {
  left: 10%;
  animation-duration: 15s;
}

.bg-squares li:nth-child(2) {
  left: 20%;
  width: 80px;
  height: 80px;
  /* 动画延迟时间 */
  animation-delay: 1s;
  /* 动画时长 */
  animation-duration: 17s;

}

.bg-squares li:nth-child(3) {
  left: 25%;
  width: 60px;
  height: 60px;
  animation-duration: 10s;
}

.bg-squares li:nth-child(4) {
  left: 35%;
  background-color: rgba(255, 255, 255, .25);
  animation-delay: 4s;
  animation-duration: 20s;
}

.bg-squares li:nth-child(5) {
  left: 40%;
  height: 70px;
  width: 70px;
  animation-duration: 7s;
}

.bg-squares li:nth-child(6) {
  left: 42%;
  height: 50px;
  width: 50px;
  animation-duration: 3s;
}

.bg-squares li:nth-child(7) {
  left: 53%;
  height: 30px;
  width: 30px;
  animation-duration: 8s;
}

.bg-squares li:nth-child(8) {
  left: 60%;
  height: 90px;
  width: 90px;
  animation-duration: 10s;
}

.bg-squares li:nth-child(9) {
  left: 64%;
  height: 50px;
  width: 50px;
  animation-duration: 7s;
}

.bg-squares li:nth-child(10) {
  left: 70%;
  height: 100px;
  width: 100px;
  animation-duration: 11s;
}

.bg-squares li:nth-child(11) {
  left: 75%;
  height: 20px;
  width: 20px;
  animation-duration: 6s;
}

.bg-squares li:nth-child(12) {
  left: 82%;
  height: 50px;
  width: 50px;
  animation-duration: 2s;
}

.bg-squares li:nth-child(13) {
  left: 90%;
  height: 20px;
  width: 20px;
  animation-duration: 4s;
}

.bg-squares li:nth-child(14) {
  left: 0;
  animation-duration: 10s;
}

.bg-squares li:nth-child(15) {
  left: 95%;
  animation-duration: 10s;
}

/* 定义动画 */
@keyframes square {
  0% {
    transform: translateY(0);
  }

  100% {
    transform: translateY(-120vh) rotate(600deg);
  }
}

.welcome_text {
  text-align: center;
  margin-bottom: 20px;
  font-weight: 700;
  background-image: -webkit-linear-gradient(right, #ffe29f,#fff);
  -webkit-background-clip: text;
   color:transparent;
}

.login_container {
  height: 100%;
  background: linear-gradient(to top left, #ffe29f, #ffa99f, #ff719a);
  overflow: hidden;
}

.login_box {
  width: 450px;
  height: 300px;
  background-color: #fff;
  border-radius: 3px;
  position: absolute;
  left: 50%;
  top: 50%;
  transform: translate(-50%, -50%);
  border: 1px solid #fff;
  z-index: 999;
}

.avatat_box {
  height: 130px;
  width: 130px;
  border: 1px solid #909399;
  border-radius: 50%;
  padding: 10px;
  box-shadow: 10px 10px 10px #909399;
  position: absolute;
  left: 50%;
  transform: translate(-50%, -50%);
  background-color: #fff;
  margin-bottom: 30px;
}

.avatat_box img {
  width: 100%;
  height: 100%;
  border-radius: 50%;
  background-color: #fff;
}

.login_form {
  margin: 100px 30px 20px 0;
}

.LoginBtns {
  display: flex;
  justify-content: space-around;
}
</style>
