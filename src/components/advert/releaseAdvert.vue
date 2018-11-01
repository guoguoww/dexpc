<template>
    <div>
      <div class="wrap">
        <div class="release">
          <div class="release-title">发布广告</div>
          <div class="release-body">
            <div class="filed clearfix">
              <div class="label fl">方向</div>
              <div class="fr">
                <select name="" v-model="subData.direction">
                  <option value="0">买入</option>
                  <option value="1">卖出</option>
                </select>
              </div>
            </div>
            <div class="filed clearfix">
              <div class="label fl">币种</div>
              <div class="fr">
                <select name="" v-model="subData.currencyCode">
                  <!--<option value="USDT">USDT</option>-->
                  <!--<option value="BTC">BTC</option>-->
                  <!--<option value="ETH">ETH</option>-->
                  <option v-for="item in $store.getters.currency.params"  :value="item.code">{{item.code}}</option>
                </select>
              </div>
            </div>
            <div class="filed clearfix">
              <div class="label fl">价格</div>
              <div class="fr">
                <input type="text" placeholder="请输入价格"  v-model="subData.price">
              </div>
            </div>
            <div class="filed clearfix">
              <div class="label fl">数量</div>
              <div class="fr">
                <input type="text" placeholder="请输入数量" v-model="subData.number">
              </div>
            </div>
            <div class="filed clearfix">
              <div class="label fl"> &nbsp </div>
              <div class="fl" style="width: 440px;padding-left: 10px;line-height: 36px;">
                <span class="fl">实际上架数量：{{subData.direction === '0'?  subData.number : (subData.number - (subData.number * freezingRate)).toFixed(2)}}</span>
                <span class="fr">冻结：{{subData.direction === '0'?  '0' : (subData.number * freezingRate).toFixed(2)}}</span>
              </div>
            </div>
            <div class="filed clearfix">
              <div class="label fl">限额</div>
              <div class="fr">
                <input type="text" placeholder="请输入限额：如120-50000" v-model="limit">
              </div>
            </div>
            <!--<div class="filed clearfix">-->
              <!--<div class="label fl">交易用户</div>-->
              <!--<div class="fr">-->
                <!--<select name="" v-model="subData.dealerStatus">-->
                  <!--<option value="0">普通认证用户</option>-->
                  <!--<option value="1">高级认证用户</option>-->
                <!--</select>-->
              <!--</div>-->
            <!--</div>-->
            <div class="filed clearfix" v-if="subData.direction === '1'">
              <div class="label fl">支付方式</div>
              <div class="fr">
                <p>选择支付方式（可以多个）</p>
                <!--<v-select multiple v-model="" :options="options"></v-select>-->
                <Select v-model="selected" filterable multiple>
                  <Option v-for="item in options" :value="item.id" :key="item.id">{{ item.label }}</Option>
                </Select>
              </div>
            </div>
            <div class="filed clearfix">
              <div class="label fl">&nbsp</div>
              <div class="fl">
                <button @click="releaseData()">确认</button>
              </div>
            </div>
          </div>
        </div>
      </div>
      <Modal
        v-model="submitCallback"
        ok-text="继续发布"
        cancel-text="查看广告列表"
        @on-ok="ok"
        @on-cancel="cancel">
        <p>发布成功</p>
      </Modal>
    </div>
</template>

<script>
  export default {
    name: "releaseAdvert",
    data() {
      return {
        selected: [],
        options: [],
        limit: '', //限额
        freezingRate: 0, //冻结率
        subData: { //初始默认上传数据
          currencyCode: 'USDT', //币种
          direction: '0', //方向 0 买入 1 卖出 ,
          maxQuota: '', //最大限额
          minQuota: '', //最小限额
          number: '' , //总数量
          // dealerStatus: '0', //交易用户
          payType: '', //收款方式
          price: '', //价格
        },
        submitCallback: false, //提交回调
      }
    },
    created() {
      this.getPayList()
      this.getFreezingRate()
    },
    mounted () {//实例创建后
      document.querySelector('#app').style.background = '#fff'
    },
    destroyed () {//组件销毁触发
      document.querySelector('#app').style.background = 'inherit'
    },
    methods: {
      getFreezingRate() { //获取冻结率
        this.$axios({
          url: "/api/advertisement/freeze_ratio",
          method: "post",
        }).then(res => {
          if(res.data.errorCode === 0) {
            this.freezingRate = Number(res.data.data)
          }
        })
      },
      getPayList() { //获取支付列表
        this.$axios({
          url: "/api/user/paymentreceived",
          method: "post",
          params: {
            start: 0,
            rows: 100,
          }
        }).then(res => {
          if(res.data.errorCode === 0) {
            for(var i=0; i<res.data.page.data.length;i++) {
              switch (res.data.page.data[i].type) {
                case '0':
                  res.data.page.data[i].type = '支付宝'
                  break;
                case '1':
                  res.data.page.data[i].type = '微信'
                  break;
                case '2':
                  res.data.page.data[i].type = '银行卡'
                  break;
              }
              this.options.push({
                id: res.data.page.data[i].id,
                label: res.data.page.data[i].type + ':' + res.data.page.data[i].account,
              })
            }
          }
        })
      },
      releaseData() { //发布
        var rsg = /^[1-9]\d*-{1}[1-9]\d*$/   //验证限额格式
        if(this.subData.price === '') {
          this.$Message.warning('请输入价格');
          return;
        }
        if(this.subData.number === '') {
          this.$Message.warning('请输入数量');
          return;
        }
        if(this.limit === '') {
          this.$Message.warning('请输入限额');
          return;
        }
        if(rsg.test(this.limit)) {
          this.subData.minQuota = this.limit.split('-')[0];
          this.subData.maxQuota = this.limit.split('-')[1];
          if(parseInt(this.subData.minQuota) > parseInt(this.subData.maxQuota)) {
            this.$Message.warning('最小限额不能大于最大限额');
            return;
          }
        }else {
          this.$Message.warning('限额格式不正确 正确格式:(例)120-50000');
          return;
        }
        if(this.subData.direction === '1') {
          if(this.selected.length === 0) {
            this.$Message.warning('请选择支付方式');
            return;
          }else {
            var a = '';
            var that = this
            this.selected.forEach(function (item,i) {
              if((i +1) === that.selected.length){
                a += item;
              }else {
                a += (item + ',');
              }
            })
            this.subData.payType = a;
          }
        }else {
          this.subData.payType = '';
        }

        this.$axios({
          url: "/api/advertisement/release",
          method: "post",
          headers: {
            "Content-Type": "application/json;charset=utf-8"
          },
          data: this.subData,
        }).then(res => {
          if(res.data.errorCode === 0) {
            this.submitCallback = true;
          }
        })
      },
      ok () {
        this.getPayList();

        this.subData.currencyCode = 'USDT'; //币种初始化
        this.subData.direction = '0'; //方向初始化
        this.limit = ''; //限额初始化
        this.subData.maxQuota = ''; //最大限额初始化
        this.subData.minQuota = ''; //最小限额初始化
        this.subData.number = ''; //总数量初始化
        this.subData.payType = ''; //收款方式初始化
        this.subData.price = ''; //价格初始化
        // this.subData.dealerStatus = '0'; //交易用户初始化
        this.selected = []; ////收款方式页面初始化
      },
      cancel () {
        this.$router.push('/advert')
      }
    },
  }
</script>

<style scoped>
  .wrap {
    min-width: 1200px;
    margin: 0 auto;
    padding: 0;
  }
  .release {
    width: 600px;
    margin: 0 auto;
  }
  .release-title {
    padding: 0 20px;
    height: 50px;
    line-height: 50px;
    border-bottom: 1px solid #efefef;
    font-size: 16px;
    color: #333;
  }
  .release-body {
    padding: 20px 0;
    box-sizing: border-box;
  }
  .filed {
    min-height: 30px;
    margin: 18px 0;
  }
  .filed .label {
    margin-right: 22px;
    width: 130px;
    line-height: 36px;
    text-align: right;
    font-size: 14px;
    color: #666;
  }
  .filed select,
  .filed input[type='text']{
    display: inline-block;
    width: 440px;
    height: 36px;
    border: 1px solid #d4d4d4;
    padding: 0 10px;
  }
  .filed select:focus {
    outline: none;
  }
  .filed p {
    height: 36px;
    line-height: 36px;
  }
  .ivu-select {
    max-width: 440px;
    width: 440px;
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
  .ivu-btn-large {
    float: left;
  }
</style>
