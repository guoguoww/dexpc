<template>
    <div>
      <div id="sign">
        <div class="sign">
          <h3>欢迎登录</h3>
          <div class="sign-list">
            <ul>
              <li class="clearfix">
                <span>账号</span>
                <input type="text" placeholder="请输入账号" v-model="upload.mobile" @keyup.enter="login()">
              </li>
              <li class="clearfix">
                <span>密码</span>
                <input type="password" placeholder="请输入密码" v-model="upload.password" @keyup.enter="login()">
              </li>
              <li class="clearfix">
                <span>验证</span>
                <div class="drag">
                  <div class="drag_bg"></div>
                  <div class="drag_text">{{confirmWords}}</div>
                  <div @mousedown="mousedownFn($event)" class="handler handler_bg"></div>
                </div>
              </li>
            </ul>
          </div>
          <div class="sign-but">
            <button @click="login()">登录</button>
            <router-link to="/forget">忘记密码？</router-link>
          </div>
          <p>还不是DSDEX的用户？ 立即<router-link to="/register"> 注册 </router-link>，在数字资产交易平台开始交易。</p>
        </div>
      </div>
    </div>
</template>

<script>
  import $ from "jquery"
  export default {
    name: "login",
    data() {
      return {
        upload: {
          mobile: '',
          password: '',
          terminal: '',//设备类型
        },
        beginClientX: 0,      /*距离屏幕左端距离*/
        mouseMoveStata: false,    /*触发拖动状态  判断*/
        maxwidth: 310,       /*拖动最大宽度，依据滑块宽度算出来的*/
        confirmWords: '请按住滑块，拖动到最右边',      /*滑块文字*/
        confirmSuccess: false              /*验证成功判断*/
      }
    },
    created () {
      //获取登陆设备
      var u = navigator.userAgent;
      if (u.indexOf("Android") > -1 || u.indexOf("Linux") > -1) {
        //安卓手机
        this.upload.terminal = 1;
      } else if (u.indexOf("iPhone") > -1) {
        //苹果手机
        this.upload.terminal = 2;
      } else {
        //wap
        this.upload.terminal = 3;
      }
    },
    mounted(){
      $('body').on('mousemove',(e) => {   //拖动，这里需要用箭头函数，不然this的指向不会是vue对象
        if(this.mouseMoveStata){
          var width = e.clientX - this.beginClientX;
          if(width>0 && width<=this.maxwidth){
            $(".handler").css({'left':width});
            $(".drag_bg").css({'width': width});
          }else if(width>this.maxwidth){
            this.successFunction();
          }
        }
      });
      $('body').on('mouseup',(e) => { //鼠标放开
        this.mouseMoveStata = false;
        var width = e.clientX - this.beginClientX;
        if(width<this.maxwidth){
          $(".handler").css({'left':0});
          $(".drag_bg").css({'width': 0});
        }
      })
    },
    methods: {
      mousedownFn:function (e) { //按下滑块函数
        this.mouseMoveStata = true;
        this.beginClientX = e.clientX;
      },
      successFunction(){  //验证成功函数
        $(".handler").removeClass('handler_bg').addClass('handler_ok_bg');
        this.confirmWords = '验证通过'
        $(".drag").css({'color': '#fff'});
        $(".handler").css({'left':this.maxwidth});
        $(".drag_bg").css({'width': this.maxwidth});
        $('body').unbind('mousemove');
        $('body').unbind('mouseup');
        this.confirmSuccess = true
      },
      login() { //登录
        if (!this.upload.mobile) {
          this.$Message.warning('请输入手机号码');
          return
        }
        if (!this.upload.password) {
          this.$Message.warning('请输入密码');
          return
        }
        if(!this.confirmSuccess) {
          this.$Message.warning('请验证后登录');
          return
        }
        this.$axios.post('api/user/login', this.upload).then(res => {
          if(res.data.errorCode === 0) {
            this.$Message.success('登录成功');
            this.$cookie.set('_auth', res.data.data.cookie);
            this.$router.push('/c2c')
          }
        })
      },
    },
  }
</script>

<style scoped>
  a:hover {
    text-decoration: underline;
  }
  #sign {
    width: 1200px;
    margin: 0 auto;
  }
  .sign {
    padding: 100px 0;
    width: 520px;
    margin: 0 auto;
  }
  h3 {
    font-size: 28px;
    font-weight: 600;
    text-align: center;
    margin-bottom: 70px;
  }
  /*登录填写内容*/
  .sign-list {
    padding: 0 33px;
  }
  .sign-list ul li {
    margin-bottom: 50px;
  }
  .sign-list ul li span {
    font-size: 14px;
    letter-spacing: 28px;
    float: left;
    line-height: 40px;
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
 /*滑动解锁css*/
  .drag{
    position: relative;
    background-color: #e8e8e8;
    width: 350px;
    height: 40px;
    line-height: 40px;
    text-align: center;
    float: right;
    margin-right: 20px;
  }
  .handler{
    position: absolute;
    top: 0;
    left: 0;
    width: 40px;
    height: 40px;
    border: 1px solid #ccc;
    cursor: move;
  }
  .handler_bg{
    background: #fff url("data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAABAAAAAQCAYAAAAf8/9hAAAAGXRFWHRTb2Z0d2FyZQBBZG9iZSBJbWFnZVJlYWR5ccllPAAAA3hpVFh0WE1MOmNvbS5hZG9iZS54bXAAAAAAADw/eHBhY2tldCBiZWdpbj0i77u/IiBpZD0iVzVNME1wQ2VoaUh6cmVTek5UY3prYzlkIj8+IDx4OnhtcG1ldGEgeG1sbnM6eD0iYWRvYmU6bnM6bWV0YS8iIHg6eG1wdGs9IkFkb2JlIFhNUCBDb3JlIDUuNS1jMDIxIDc5LjE1NTc3MiwgMjAxNC8wMS8xMy0xOTo0NDowMCAgICAgICAgIj4gPHJkZjpSREYgeG1sbnM6cmRmPSJodHRwOi8vd3d3LnczLm9yZy8xOTk5LzAyLzIyLXJkZi1zeW50YXgtbnMjIj4gPHJkZjpEZXNjcmlwdGlvbiByZGY6YWJvdXQ9IiIgeG1sbnM6eG1wTU09Imh0dHA6Ly9ucy5hZG9iZS5jb20veGFwLzEuMC9tbS8iIHhtbG5zOnN0UmVmPSJodHRwOi8vbnMuYWRvYmUuY29tL3hhcC8xLjAvc1R5cGUvUmVzb3VyY2VSZWYjIiB4bWxuczp4bXA9Imh0dHA6Ly9ucy5hZG9iZS5jb20veGFwLzEuMC8iIHhtcE1NOk9yaWdpbmFsRG9jdW1lbnRJRD0ieG1wLmRpZDo0ZDhlNWY5My05NmI0LTRlNWQtOGFjYi03ZTY4OGYyMTU2ZTYiIHhtcE1NOkRvY3VtZW50SUQ9InhtcC5kaWQ6NTEyNTVEMURGMkVFMTFFNEI5NDBCMjQ2M0ExMDQ1OUYiIHhtcE1NOkluc3RhbmNlSUQ9InhtcC5paWQ6NTEyNTVEMUNGMkVFMTFFNEI5NDBCMjQ2M0ExMDQ1OUYiIHhtcDpDcmVhdG9yVG9vbD0iQWRvYmUgUGhvdG9zaG9wIENDIDIwMTQgKE1hY2ludG9zaCkiPiA8eG1wTU06RGVyaXZlZEZyb20gc3RSZWY6aW5zdGFuY2VJRD0ieG1wLmlpZDo2MTc5NzNmZS02OTQxLTQyOTYtYTIwNi02NDI2YTNkOWU5YmUiIHN0UmVmOmRvY3VtZW50SUQ9InhtcC5kaWQ6NGQ4ZTVmOTMtOTZiNC00ZTVkLThhY2ItN2U2ODhmMjE1NmU2Ii8+IDwvcmRmOkRlc2NyaXB0aW9uPiA8L3JkZjpSREY+IDwveDp4bXBtZXRhPiA8P3hwYWNrZXQgZW5kPSJyIj8+YiRG4AAAALFJREFUeNpi/P//PwMlgImBQkA9A+bOnfsIiBOxKcInh+yCaCDuByoswaIOpxwjciACFegBqZ1AvBSIS5OTk/8TkmNEjwWgQiUgtQuIjwAxUF3yX3xyGIEIFLwHpKyAWB+I1xGSwxULIGf9A7mQkBwTlhBXAFLHgPgqEAcTkmNCU6AL9d8WII4HOvk3ITkWJAXWUMlOoGQHmsE45ViQ2KuBuASoYC4Wf+OUYxz6mQkgwAAN9mIrUReCXgAAAABJRU5ErkJggg==") no-repeat center;
  }
  .handler_ok_bg{
    background: #fff url("data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAABAAAAAQCAYAAAAf8/9hAAAAGXRFWHRTb2Z0d2FyZQBBZG9iZSBJbWFnZVJlYWR5ccllPAAAA3hpVFh0WE1MOmNvbS5hZG9iZS54bXAAAAAAADw/eHBhY2tldCBiZWdpbj0i77u/IiBpZD0iVzVNME1wQ2VoaUh6cmVTek5UY3prYzlkIj8+IDx4OnhtcG1ldGEgeG1sbnM6eD0iYWRvYmU6bnM6bWV0YS8iIHg6eG1wdGs9IkFkb2JlIFhNUCBDb3JlIDUuNS1jMDIxIDc5LjE1NTc3MiwgMjAxNC8wMS8xMy0xOTo0NDowMCAgICAgICAgIj4gPHJkZjpSREYgeG1sbnM6cmRmPSJodHRwOi8vd3d3LnczLm9yZy8xOTk5LzAyLzIyLXJkZi1zeW50YXgtbnMjIj4gPHJkZjpEZXNjcmlwdGlvbiByZGY6YWJvdXQ9IiIgeG1sbnM6eG1wTU09Imh0dHA6Ly9ucy5hZG9iZS5jb20veGFwLzEuMC9tbS8iIHhtbG5zOnN0UmVmPSJodHRwOi8vbnMuYWRvYmUuY29tL3hhcC8xLjAvc1R5cGUvUmVzb3VyY2VSZWYjIiB4bWxuczp4bXA9Imh0dHA6Ly9ucy5hZG9iZS5jb20veGFwLzEuMC8iIHhtcE1NOk9yaWdpbmFsRG9jdW1lbnRJRD0ieG1wLmRpZDo0ZDhlNWY5My05NmI0LTRlNWQtOGFjYi03ZTY4OGYyMTU2ZTYiIHhtcE1NOkRvY3VtZW50SUQ9InhtcC5kaWQ6NDlBRDI3NjVGMkQ2MTFFNEI5NDBCMjQ2M0ExMDQ1OUYiIHhtcE1NOkluc3RhbmNlSUQ9InhtcC5paWQ6NDlBRDI3NjRGMkQ2MTFFNEI5NDBCMjQ2M0ExMDQ1OUYiIHhtcDpDcmVhdG9yVG9vbD0iQWRvYmUgUGhvdG9zaG9wIENDIDIwMTQgKE1hY2ludG9zaCkiPiA8eG1wTU06RGVyaXZlZEZyb20gc3RSZWY6aW5zdGFuY2VJRD0ieG1wLmlpZDphNWEzMWNhMC1hYmViLTQxNWEtYTEwZS04Y2U5NzRlN2Q4YTEiIHN0UmVmOmRvY3VtZW50SUQ9InhtcC5kaWQ6NGQ4ZTVmOTMtOTZiNC00ZTVkLThhY2ItN2U2ODhmMjE1NmU2Ii8+IDwvcmRmOkRlc2NyaXB0aW9uPiA8L3JkZjpSREY+IDwveDp4bXBtZXRhPiA8P3hwYWNrZXQgZW5kPSJyIj8+k+sHwwAAASZJREFUeNpi/P//PwMyKD8uZw+kUoDYEYgloMIvgHg/EM/ptHx0EFk9I8wAoEZ+IDUPiIMY8IN1QJwENOgj3ACo5gNAbMBAHLgAxA4gQ5igAnNJ0MwAVTsX7IKyY7L2UNuJAf+AmAmJ78AEDTBiwGYg5gbifCSxFCZoaBMCy4A4GOjnH0D6DpK4IxNSVIHAfSDOAeLraJrjgJp/AwPbHMhejiQnwYRmUzNQ4VQgDQqXK0ia/0I17wJiPmQNTNBEAgMlQIWiQA2vgWw7QppBekGxsAjIiEUSBNnsBDWEAY9mEFgMMgBk00E0iZtA7AHEctDQ58MRuA6wlLgGFMoMpIG1QFeGwAIxGZo8GUhIysmwQGSAZgwHaEZhICIzOaBkJkqyM0CAAQDGx279Jf50AAAAAABJRU5ErkJggg==") no-repeat center;
  }
  .drag_bg{
    background-color: #7ac23c;
    height: 40px;
    width: 0;
  }
  .drag_text{
    position: absolute;
    top: 0;
    width: 350px;
    -moz-user-select: none;
    -webkit-user-select: none;
    user-select: none;
    -o-user-select:none;
    -ms-user-select:none;
  }
  /*登录按钮*/
  .sign-but {
    padding: 0 124px 45px;
  }
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
  .sign-but a {
    padding-left: 33px;
  }
  p {
    padding-left: 33px;
    color: #201a40;
    font-size: 14px;
    padding-bottom: 38px;
  }
  a {
    font-size: 14px;
    color: #5b8fef;
  }

</style>
