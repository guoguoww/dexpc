<template>
    <div>
      <div class="wrap">
        <div class="senior">
          <div class="senior-title">高级认证</div>
          <div class="senior-body">
            <div class="filed clearfix">
              <div class="label fl">身份证正面照</div>
              <div class="fr pics">
                <input type="file" accept="image/*" @change="changImg('idCardZ', $event)">
                <img id="idCardZ" src="" alt="">
                <Icon  type="ios-add" />
              </div>
            </div>
            <div class="filed clearfix">
              <div class="label fl">身份证反面照</div>
              <div class="fr pics">
                <input type="file" accept="image/*" @change="changImg('idCardF', $event)">
                <img id="idCardF" src="" alt="">
                <Icon  type="ios-add" />
              </div>
            </div>
            <div class="filed clearfix">
              <div class="label fl">手持身份证照</div>
              <div class="fr pics">
                <input type="file" accept="image/*" @change="changImg('handheldId', $event)">
                <img id="handheldId" src="" alt="">
                <Icon  type="ios-add" />
              </div>
            </div>
            <div class="filed clearfix">
              <div class="fr">
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
    name: "seniorCertification",
    data() {
      return {
        idCardZ: '', //身份证正面
        idCardF: '', //身份证反面
        handheldId: '' //手持身份证照
      }
    },
    mounted () {//实例创建后
      document.querySelector('#app').style.background = '#fff'
    },
    destroyed () {//组件销毁触发
      document.querySelector('#app').style.background = 'inherit'
    },
    methods: {
      changImg(id, e) { //上传图片回显 //企业/组织营业执照 证件正反面
        var that = this;
        var id = id;
        for (var i = 0; i < e.target.files.length; i++) {
          var file = e.target.files.item(i);
          if (!(/^image\/.*$/i.test(file.type))) {
            continue; //不是图片 就跳出这一次循环
          }
          if(e.target.files[i].size > 5242880) { //大小不能超过5M
            this.$Message.warning('海报大小不能超过5M');
            continue
          }
          //实例化FileReader API
          var freader = new FileReader();
          freader.readAsDataURL(file);
          freader.onload = function(e) {
            document.getElementById(id).setAttribute('src',e.target.result);
            if(id === 'idCardZ') {
              that.idCardZ = e.target.result
            }
            if(id === 'idCardF') {
              that.idCardF = e.target.result
            }
            if(id === 'handheldId') {
              that.handheldId = e.target.result
            }
          }
        }
      },
      submitData() { //提交信息
        if(this.idCardZ === '') {
          this.$Message.warning('请上传身份证正面照片');
          return;
        }
        if(this.idCardF === '') {
          this.$Message.warning('请上传身份证反面照片');
          return;
        }
        if(this.handheldId === '') {
          this.$Message.warning('请上传手持身份证照片');
          return;
        }
        this.$axios({
          url: "/api/user/certificationadvanced",
          method: "post",
          headers: {
            "Content-Type": "application/json;charset=utf-8"
          },
          data: {
            certificateFrontPhoto: this.idCardZ, //正面
            certificateBackPhoto: this.idCardF, //反面
            certificateHandPhoto: this.handheldId, //手持
          }
        }).then(res => {
          if(res.data.errorCode === 0) {
            this.$Message.success("认证信息已提交 请等待审核结果")
            this.$router.push('/user')//跳转个人中心
          }
        })
          .catch(err => {});
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
  .senior {
    width: 600px;
    margin: 0 auto;
  }
  .senior-title {
    padding: 0 20px;
    height: 50px;
    line-height: 50px;
    border-bottom: 1px solid #efefef;
    font-size: 16px;
    color: #333;
  }
  .senior-body {
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
    line-height: 100px;
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
  .pics {
    width: 280px;
    height: 150px;
    margin-bottom: 15px;
    background: #FAFAFA;
    border: 1px dashed #d1d1d1;
    position: relative;
  }
  .pics input {
    width: 100%;
    height: 100%;
    position: absolute;
    z-index: 201;
    background: #FA322B;
    border-radius: 2px;
    color: #fff;
    cursor: pointer;
    font-style: normal;
    left: 0;
    top: 0;
    opacity: 0;
  }
  .pics img {
    position: absolute;
    left: 0;
    top: 0;
    width: 100%;
    height: 100%;
  }
  .pics i {
    width: 30px;
    height: 30px;
    position: absolute;
    left: calc(50% - 25px);
    top: calc(50% - 25px);
    font-size: 50px;
    color: #d1d1d1
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
