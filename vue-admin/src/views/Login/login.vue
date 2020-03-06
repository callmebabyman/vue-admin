<template>
  <div id="login">
    <div class="login_form">
      <ul class="menu_tab">
        <li v-for="item in menu_tab" :key="item.id" :class="{'current':item.current}" @click="toggleMneu(item)">
          {{item.txt}}
        </li>
      </ul>


      <el-form :model="ruleForm" status-icon :rules="rules" ref="ruleForm" class="login_form" size="	medium">
        <el-form-item prop="username">
          <label>邮箱</label>
          <el-input type="text" v-model="ruleForm.username" autocomplete="off"></el-input>
        </el-form-item>

        <el-form-item prop="password">
          <label>密码</label>
          <el-input type="text" v-model="ruleForm.password" autocomplete="off" minlength="6" maxlength="12"></el-input>
        </el-form-item>

        <el-form-item prop="passwords" class="item-from" v-show="model ==='register'">
          <label>重复密码</label>
          <el-input type="text" v-model="ruleForm.passwords" autocomplete="off" minlength="6" maxlength="12"></el-input>
        </el-form-item>

        <el-form-item prop="code" class="item-from">
          <label>验证码</label>
          <el-row :gutter="10">
            <el-col :span="15">
                <el-input v-model.number="ruleForm.code" minlength="6" maxlength="6" ></el-input>
            </el-col>
            <el-col :span="9">
                <el-button type="success" class="block">获取验证码</el-button>
            </el-col>
          </el-row>

        </el-form-item>

        <el-form-item>
          <label> </label>
          <el-button type="danger" @click="submitForm('ruleForm')" class="login-btn block" size="small">登录</el-button>
        </el-form-item>
      </el-form>
    </div>
  </div>
</template>

<script>
import {stripscript ,validateEmail} from '../../utils/validate'

export default {
  name: 'login',
  data () {
    var checkcode = (rule, value, callback) => {

      let reg = /^[a-z0-9]{6}$/
      if (!value) {
        return callback(new Error('验证码不能为空'))
      }else if(!reg.test(value)) {
         return  callback(new Error('验证码格式有误'));
      }
      else{
        callback();
      }

    }
    var validateUsername = (rule, value, callback) => {

      let reg = /^(?!\D+$)(?![^a-zA-Z]+$)\S{6,20}$/

      if (value === '') {
        return  callback(new Error('请输入用户名'));
      } else if(!reg.test(value)) {
        return callback(new Error('用户名格式有误'));
      }
        else{
        callback();
    }
      }

    var validatePassword = (rule, value, callback) => {
      let reg = /^(?!\D+$)(?![^a-zA-Z]+$)\S{6,20}$/

      console.log(stripscript(value))

      //过滤后的数据
      this.ruleForm.password=stripscript(value)
      value=this .ruleForm.password


      if (value === '') {
        callback(new Error('请输入密码'))
      } else if (!reg.test(value)) {
        callback(new Error('密码格式有误！密码需为6-12位的字母+数字'))
      } else {
        callback()
      }
    }

    var validatePasswords = (rule, value, callback) => {
      // console.log(value)
      // console.log( this.ruleForm.password)
      //如果模块值为login ，直接通过
      if (this.model==='login'){
        callback()
      }
      //过滤后的数据
      this.ruleForm.passwords=stripscript(value)
      value=this.ruleForm.passwords
      if (value === '') {
        callback(new Error('请再次输入密码'))
      }
      else if (value !== this.ruleForm.password) {
        callback(new Error('重复密码不正确'))
      }
      else {
        callback()
      }
    }

    return {
      ruleForm: {
        username: '',
        password: '',
        code: '',
        passwords:''
      },
      model:'login',
      rules: {
        //失去焦点触发
        username: [
          { validator: validateUsername, trigger: 'blur' }
        ],
        password: [
          { validator: validatePassword, trigger: 'blur' }
        ],
        passwords: [
          { validator: validatePasswords, trigger: 'blur' }
        ],
        code: [
          { validator: checkcode, trigger: 'blur' }
        ]
      },

      menu_tab: [
        { txt: '登录', current: true ,type:'login' },
        { txt: '注册', current: false ,type: 'register'},
      ],
      isActive: true


    }
  },
  created () {
  },
  mounted () {
  },
  methods: {
    toggleMneu (data) {
      console.log(data)

      this.menu_tab.forEach((elem ,index)=>{
        elem.current=false
      });
      data.current=true
      this.model=data.type
    },

    submitForm (formName) {
      this.$refs[formName].validate((valid) => {
        if (valid) {
          alert('submit!')
        } else {
          console.log('error submit!!')
          return false
        }
      })
    },
    resetForm (formName) {
      this.$refs[formName].resetFields()
    }
  }
}
</script>

<style lang="scss" scoped>
  #login {
    height: 100vh;
    background-color: #5f36ff;
  }

  .login_form {
    width: 330px;
    margin: auto;
  }

  .menu_tab {
    text-align: center;

    li {
      display: inline-block;
      width: 88px;
      line-height: 36px;
      font-size: 15px;
      color: #e4e4e4;
      border-radius: 3px;
      cursor: pointer;
    }

    .current {
      background-color: rgba(0, 0, 0, .1);
    }
  }

  .login-form {
    margin-top: 29px;

    label {
      display: block;
      margin-bottom: 3px;
      font-size: 14px;
      color: #e4e4e4;
    }
  }

  .item-from {
    margin-bottom: 13px;
  }

  .block {
    display: block;
    width: 100%;
  }
</style>
