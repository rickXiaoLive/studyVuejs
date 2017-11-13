<template>
    <div class="login">
      <h1>{{ count }}</h1>
      <h1>{{ doneTodos }}</h1>
      <button @click="increment">jia</button>
      <button @click="incrementBy({num:2})">jia</button>
      <div id="loginBox">
        <el-form :model="user" status-icon :rules="rules" ref="ruleForm2" label-width="0px" class="demo-ruleForm">
          <el-form-item prop="email">
            <el-input placeholder="邮箱"  type="text" v-model="user.email" auto-complete="on"></el-input>
          </el-form-item>
          <el-form-item prop="password">
            <el-input placeholder="密码"  type="password" v-model="user.password" auto-complete="on"></el-input>
          </el-form-item>
          <el-form-item prop="identifyingCode">
            <el-input placeholder="验证码"  type="text" v-model="user.identifyingCode" auto-complete="on"></el-input>
          </el-form-item>
          <el-form-item>
            <el-button type="primary" @click="submitForm('ruleForm2')">提交</el-button>
            <el-button @click="resetForm('ruleForm2')">重置</el-button>
          </el-form-item>
        </el-form>
      </div>
    </div>
</template>

<script>
  import { mapState,mapGetters,mapMutations } from 'vuex'

  export default {
    data() {
      const checkIdentifyingCode = (rule, value, callback) => {
        if (!value) {
          return callback(new Error('验证码不能为空'));
        }
        axios.post()
        callback();
      };
      const validateEmail = (rule, value, callback) => {
        if (value === '') {
          callback(new Error('请输邮箱'));
        }else if (!/^[\w!#$%&'*+/=?^_`{|}~-]+(?:\.[\w!#$%&'*+/=?^_`{|}~-]+)*@(?:[\w](?:[\w-]*[\w])?\.)+[\w](?:[\w-]*[\w])?$/.test(value)){
          callback(new Error('请输正确的邮箱'));
        } else {
          callback();
        }
      };

      return {
        user: {
          email: '',
          password: '',
          identifyingCode: ''
        },
        rules: {
          email: [
            { validator: validateEmail, trigger: 'blur' }
          ],
          password: [
            { required: true, message: '请输入密码', trigger: 'blur' },
            { min: 3, max: 5, message: '长度在 3 到 5 个字符', trigger: 'blur' }
          ],
          identifyingCode: [
            { validator: checkIdentifyingCode, trigger: 'blur' }
          ]
        }
      };
    },
    methods: {
      submitForm(formName) {
        this.$refs[formName].validate((valid) => {
          if (valid) {
            alert('submit!');
          } else {
            console.log('error submit!!');
            return false;
          }
        });
      },
      resetForm(formName) {
        this.$refs[formName].resetFields();
      },
      ...mapMutations(['increment','incrementBy'])
    },
    computed: {
      ...mapState(['count']),
      ...mapGetters(['doneTodos'])
    }
  }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  .login{
    height: 100%;
    background: #7e8c8d;

  }
  #loginBox{
    width: 300px;
    position: absolute;
    top: 300px;
    left: 50%;
    margin-left: -150px;
    background:rgba(255,255,255,0.8);
    border-radius: 8px;
    padding: 30px 30px 0 30px;
  }
</style>
