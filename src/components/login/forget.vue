<template>
    <div>
      <div id="sign">
        <div class="sign">
          <h3>忘记密码</h3>
          <!--提交填写内容-->
          <div class="sign-list">
            <ul>
              <li class="clearfix">
                <span>手机号码</span>
                <input type="text" placeholder="" v-model="upload.mobile">
              </li>
              <li class="yzm clearfix">
                <span>短信验证码</span>
                <input type="password" placeholder="" v-model="upload.smsValidCode" @keyup.enter="forget()">
                <button @click="getCode()" :disabled='disabled' :class="{disabled: disabled}">{{isNaN(codetext) ? codetext : codetext + '秒'}}</button>
              </li>
              <li class="clearfix">
                <span>新密码</span>
                <input type="password" placeholder="输入密码"  v-model="upload.password" @keyup.enter="forget()">
              </li>
              <li class="clearfix">
                <span>确认密码</span>
                <input type="password" placeholder="确认密码" v-model="confirmPwd" @keyup.enter="forget()">
              </li>
            </ul>
          </div>
          <!--按钮区域-->
          <div class="sign-but">
            <button @click="forget()">确定</button>
          </div>
        </div>
      </div>
    </div>
</template>

<script>
  export default {
    name: "forget",
    data() {
      return {
        upload: {
          mobile: "", //手机
          smsValidCode: "", //验证码
          password: "", //密码
        },
        disabled: false, //验证码按钮状态
        timer: null, //验证码定时器
        codetext: '获取验证码', //验证码文字信息
        postCode: {
          mobile: '', //手机号
          type: 'mtpwd'
        },
        confirmPwd: '', //确认密码
      }
    },
    methods: {
      getCode() { //获取短信验证码
        if (!this.upload.mobile) {
          this.$Message.error('请输入手机号码');
          return;
        }
        this.postCode.mobile = this.upload.mobile;
        this.$axios({
          url: "/api/sms/validate_code",
          method: "post",
          params: this.postCode
        }).then(res=>{
          if (res.data.errorCode === 0) {
            this.$Message.success('发送成功');
            this.disabled = true;
            this.codetext = 60;
            this.timer = setInterval(()=>{
              if(this.codetext > 0 && this.codetext <= 60 > 0 && this.codetext <= 60){
                this.codetext--
              }else{
                this.disabled = false
                this.codetext = '获取验证码'
                clearInterval(this.timer)
                this.timer = null
              }
            },1000)
          }
        })
      },
      forget() { //忘记密码
        if (!this.upload.password) {
          this.$Message.warning("请输入密码");
          return;
        }
        if (!this.upload.smsValidCode) {
          this.$Message.warning("请输入验证码");
          return;
        }
        if (this.upload.password !== this.confirmPwd) {
          this.$Message.warning("两次输入密码不一致");
          return;
        }
        this.$axios
          .post("/api/user/modify_password", this.upload)
          .then(res => {
            if (res.data.errorCode === 0) {
              this.$Message.success("密码修改成功");
              this.$router.push('/login')
            }
          })
          .catch(err => {});
      },
    },
  }
</script>

<style scoped>
  #sign {
    width: 1200px;
    margin: 0 auto;
  }
  .sign {
    padding: 50px 0;
    width: 520px;
    margin: 0 auto;
  }
  h3 {
    font-size: 28px;
    font-weight: 600;
    text-align: center;
    margin-bottom: 70px;
  }
  /*提交填写内容*/
  .sign-list ul li {
    margin-bottom: 20px;
  }
  /*获取验证码按钮*/
  .yzm {
    position: relative;
  }
  .yzm button {
    height: 30px;
    background: #fff;
    border: 0;
    outline: 0;
    position: absolute;
    right: 30px;
    top: 4px;
    padding-left: 10px;
    border-left: 1px solid #000;
    cursor: pointer;
  }
  .sign-list ul li span {
    display: inline-block;
    width: 130px;
    font-size: 14px;
    letter-spacing: 2px;
    float: left;
    line-height: 40px;
    text-align: right;
    margin-right: 20px;
  }
  .sign-list ul li input {
    width: 350px;
    height: 38px;
    border: 1px solid #868587;
    border-radius: 4px;
    padding-left: 10px;
    outline: none;
    color: #aaa;
    float: left;
  }
  .sign-but {
    padding: 0 149px 45px;
  }
  /*按扭区域*/
  .sign-but button {
    width: 130px;
    height: 40px;
    background: #5b8fef;
    outline: none;
    cursor: pointer;
    border-radius: 4px;
    border: 0;
    color: #c8d8fc;
  }
</style>
