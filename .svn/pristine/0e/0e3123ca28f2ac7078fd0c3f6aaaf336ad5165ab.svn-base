<template>
    <div>
      <div class="wrap">
        <div class="identity">
          <div class="identity-title">实名认证</div>
          <div class="identity-body">
            <div class="filed clearfix">
              <div class="label fl">姓名</div>
              <div class="fl">
                <input type="text" v-model="userName" @keyup.enter="submitData()">
              </div>
            </div>
            <div class="filed clearfix">
              <div class="label fl">有效身份证</div>
              <div class="fl">
                <input type="text" v-model="userId" @keyup.enter="submitData()">
              </div>
            </div>
            <div class="filed clearfix">
              <div class="label fl">&nbsp</div>
              <div class="fl">
                <button @click="submitData()">确认</button>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
</template>

<script>
  export default {
    name: "realName",
    data() {
      return {
        userName: '',
        userId: '',
      }
    },
    mounted () {//实例创建后
      document.querySelector('#app').style.background = '#fff'
    },
    destroyed () {//组件销毁触发
      document.querySelector('#app').style.background = 'inherit'
    },
    methods: {
      submitData() { //实名认证
        if (this.userName === '') {
          this.$Message.error('请输入身份证姓名');
          return;
        }
        if(!/^[\u4e00-\u9fa5]+$/gi.test(this.userName)) {
          this.$Message.error('姓名只能输入中文');
          return;
        }
        if (this.userId === '') {
          this.$Message.error('请输入身份证号码');
          return;
        }
        if(!/(^\d{15}$)|(^\d{18}$)|(^\d{17}(\d|X|x)$)/.test(this.userId)) {
          this.$Message.error('身份证号码不合法');
          return;
        }
        this.$axios({
          url: "/api/user/certificationgeneral",
          method: "post",
          headers: {
            "Content-Type": "application/json;charset=utf-8"
          },
          data: {
            chnName: this.userName,
            idCard: this.userId,
          }
        }).then(res => {
          if (res.data.errorCode === 0) {
            this.$Message.success("认证信息已提交 请等待审核结果")
            this.$router.push('/user')//跳转个人中心
          }
        })
          .catch(err => {
          });
      },

    },
  }
</script>

<style scoped>
  .wrap {
    min-width: 1200px;
    margin: 0 auto;
    padding: 0;
  }
  .identity {
    width: 600px;
    margin: 0 auto;
  }
  .identity-title {
    padding: 0 20px;
    height: 50px;
    line-height: 50px;
    border-bottom: 1px solid #efefef;
    font-size: 16px;
    color: #333;
  }
  .identity-body {
    padding: 20px 50px;
    box-sizing: border-box;
  }
  .filed {
    min-height: 30px;
    margin: 18px 0;
  }
  .filed .label {
    margin-right: 22px;
    width: 130px;
    line-height: 30px;
    text-align: right;
    font-size: 14px;
    color: #666;
  }
  .filed input[type='text'] {
    display: inline-block;
    width: 280px;
    height: 28px;
    border: 1px solid #d4d4d4;
    padding: 0 10px;
  }
  button {
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
