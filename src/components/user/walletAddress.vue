<template>
    <div>
      <div class="wrap">
        <div class="container">
          <div class="dw-title">
            地址管理
          </div>
          <div class="dw-body f-cb">
            <div class="f-fl">
              <div class="depositBox">
                <div class="asset-info">
                  <div class="asset-content f-cb">
                    <p id="select" @click="selClick"><span ref="selBZ">选择币种</span><span class="bt_arrow"></span></p>
                    <ul class="selInfo" v-show="selList">
                      <!--<li @click="selText(item)" v-for="(item, index) in selInfos" :key="index">{{item}}</li>-->
                      <li v-for="item in $store.getters.currency.params" @click="selText(item.code)">{{item.code}}</li>
                    </ul>
                  </div>
                  <div>
                    <div class="speBox">
                      <div class="importants">
                        <strong>注意: </strong>
                        <p>• 充币提币请勿试用绑定地址以外的地址进行转账，不然您的资产将可能无法追回。</p>
                      </div>
                      <div class="withdrawal">
                        <div class="f-cb">
                          <strong>
                            钱包地址
                          </strong>
                        </div>
                        <div class="selectBoxFull ">
                          <input type="text" style="background: #fff" v-model="subData.address" >
                        </div>
                      </div>
                      <div class="withdrawal">
                        <div class="f-cb">
                          <strong>
                            备注
                          </strong>
                        </div>
                        <div class="selectBoxFull ">
                          <input type="text" style="background: #fff" v-model="subData.remark">
                        </div>
                      </div>
                      <div class="submitBtn">
                        <button class="btn btn-orange btn-block" @click="submitAddress">确认</button>
                      </div>
                    </div>
                    <div class="tips">
                      <strong>提示</strong>
                      <p>• 绑定充币提币地址，您的数字资产将更加安全。</p>
                    </div>
                  </div>
                </div>
              </div>
            </div>
            <div class="f-fr">
              <div class="lists deposit">
                <div class="lists-title">
                  地址列表
                </div>
                <ul class="lists-body">
                  <li class="th clearfix">
                    <div class="name">币种</div>
                    <div class="address">钱包地址</div>
                    <div class="status">备注</div>
                    <div class="">操作</div>
                  </li>
                  <li class="clearfix" v-if="addressList.data.length >= 0" v-for="item in addressList.data">
                    <div class="name">{{item.currencyCode}}</div>
                    <div class="address">{{item.address}}</div>
                    <div class="status">{{!!item.remark? item.remark : '无'}}</div>
                    <div class="">
                      <b class="btn" @click="removeAddress(item.id)">解绑</b>
                    </div>
                  </li>
                </ul>
                <div class="nodata" v-if="addressList.data.length === 0">
                  <img src="../../../static/imgs/record.png">
                  <p class="" style="margin-left:-4px;">无记录</p>
                </div>
                <div style="text-align: center;margin-top: 50px" v-if="addressList.total > pages.rows">
                  <Page :total="addressList.total" :page-size="pages.rows" size="small" show-total  @on-change="pageChange"/>
                </div>
              </div>
            </div>
          </div>
        </div>

      </div>
    </div>
</template>

<script>
  export default {
    name: "walletAddress",
    data() {
      return {
        selList: false,
        // selInfos: ['USDT', 'BTC', 'ETH'],
        subData: {
          currencyCode: '', //币种
          address: '', //地址
          remark: '', //备注
        },
        pages: {
          start: 0,
          rows: 10,
        },
        addressList: {
          data: [],
          total: 0
        },
      }
    },
    created() {
      this.getAddressList()
    },
    methods: {
      selClick(){//点击选择币种
        this.selList = this.selList? false : true;
      },
      selText(txt){//选中某一项内容
        this.$refs.selBZ.innerHTML = txt;
        this.selList = false;
        this.subData.currencyCode = txt;
      },
      submitAddress() { //绑定地址
        if(this.subData.currencyCode === '') {
          this.$Message.warning('请选择币种');
          return;
        }
        if(this.subData.address === '') {
          this.$Message.warning('请填写钱包地址');
          return;
        }
        this.$axios({
          url: '/api/wallet/saveaddress',
          method: "post",
          data: this.subData
        }).then(res => {
          if(res.data.errorCode === 0) {
            this.$Message.success('绑定成功');
            this.getAddressList()
            // 初始化数据
            this.$refs.selBZ.innerHTML = '选择币种';
            this.selList = false;
            this.subData.currencyCode = '';
            this.subData.address = '';
            this.subData.remark = ''
          }
        })
      },
      getAddressList() { //查询地址列表
        this.$axios({
          url: '/api/wallet/queryaddress',
          method: "post",
          data: this.pages
        }).then(res => {
          if(res.data.errorCode === 0) {
            this.addressList.data = res.data.page.data;
            this.addressList.total =  res.data.page.total;
          }
        })
      },
      pageChange(page) { //支付列表分页
        this.pages.start = this.pages.rows * (page-1);
        this.getAddressList()
      },
      removeAddress(id) { //解除绑定地址
        if(confirm("确认解除绑定？")) {
          this.$axios({
            url: '/api/wallet/deladdress',
            method: "post",
            data: {
              id: id
            }
          }).then(res => {
            if(res.data.errorCode === 0) {
              this.$Message.success('解除成功');
              this.getAddressList();
            }
          })
        }
      },
    }
  }
</script>

<style scoped>
  .wrap {
    background: #f5f4fc;
    min-width: 1200px;
    padding-bottom: 30px;
  }
  .container {
    width: 1200px;
    margin: 0 auto;
    padding: 0;
  }
  .dw-title {
    width: 100%;
    height: 40px;
    line-height: 40px;
    font-size: 18px;
    color: #333;
    padding: 10px 0;
    margin-bottom: 10px;
  }
  .dw-body {
    background-color: #fff;
    border-radius: 10px !important;
    min-height: 700px;
  }
  .dw-body > .f-fl, .dw-body > .f-fr {
    width: 638px;
    min-height: 200px;
  }
  .f-fl {
    float: left;
  }
  .f-fr {
    float: right;
  }
  .depositBox .asset-info {
    padding: 38px 58px 20px;
  }
  .f-cb:after, .f-cbli li:after {
    display: block;
    clear: both;
    visibility: hidden;
    height: 0;
    overflow: hidden;
    content: ".";
  }
  #select, #selAddr {
    white-space: nowrap;
    overflow: hidden;
    width: 100%;
    padding-right: 1.5em;
    border-radius: 3px;
    color: #a7b7da;
    height: 32px;
    line-height: 32px;
    font-size: 1rem;
    border: 1px solid rgb(233, 238, 248);
    cursor: pointer;
    position: relative;
    padding-left: 10px;
  }
  #select span.bt_arrow, #selAddr span.bt_arrow {
    border: .4rem solid transparent;
    border-top-color: #a7b7da;
    cursor: pointer;
    width: 0;
    height: 0;
    position: absolute;
    right: .5rem;
    top: .8rem;
  }
  .asset-content, .asset-address {
    position: relative;
    min-width: 100%;
    margin-bottom: 10px;
  }
  ul.selInfo {
    width: 100%;
    padding: .4em;
    border: 1px solid #D5D5D5;
    box-shadow: 0px 3px 5px #ddd;
    position: absolute;
    display: block;
    z-index: 1005;
    top: 32px;
    left: 0px;
    border-radius: 3px;
    opacity: 1;
    height: auto;
    background: #ffffff;
    color: #222222;
  }
  ul.selInfo li {
    line-height: 35px ;
    border: 0 none;
    cursor: pointer;
    margin: 1px 0;
    padding: .125em .25em ;
    text-align: left;
  }
  .speBox {
    background: #e9eef8;
    padding: 15px 20px;
  }
  .speBox>div {
    margin: 20px 0 10px;
  }
  .speBox strong {
    font-weight: bold;
    color: #000;
    font-size: 12px;
  }
  .importants {
    font-size: 12px;
  }
  .importants p {
    padding-top: 0.3rem;
    line-height: 1.5;
  }
  .dw-body > .f-fr {
    width: 538px;
  }
  .lists {
    line-height: 30px;
  }
  .lists .lists-title {
    background: #e9eef8;
    font-size: 12px;
    color: #999;
    border: none;
    margin-top: 38px;
    margin-right: 40px;
    border-radius: 4px;
  }
  .lists.deposit .lists-title{
    padding: 0 10px;
  }
  .lists .lists-title, .lists .lists-body li {
    line-height: 30px;
    border-bottom: 1px solid #f0f0f0;
  }
  .lists-body {
    margin-right: 40px;
  }
  .lists .lists-body li.th {
    background: #fafafa;
    color: #999;
    padding: 0;
  }
  .lists .lists-body li > div {
    float: left;
    font-size: 12px;
    color: #666;
    text-align: left;
    padding: 0 14px;
    white-space: nowrap;
    overflow: hidden;
    text-overflow: ellipsis;
  }
  .lists .lists-body li.th > div {
    border-right: 1px solid #e6e6e6;
    padding: 0 12px;
    color: #999;
  }
  .lists .lists-body li.th > div:last-of-type {
    border: none;
  }
  .lists .lists-body li > div.status {
    width: 100px;
  }
  .lists .lists-body li > div.address {
    width: 260px;
  }
  .lists .lists-body li > div.name {
    width: 60px;
  }
  .lists .lists-body li > div b.btn {
    display: inline-block;
    height: 24px;
    line-height: 24px;
    vertical-align: middle;
    font-weight: normal;
    cursor: pointer;
    text-decoration: line-through;
  }
  .nodata {
    text-align: center;
    padding: 20px;
  }
  .nodata img {
    width: 60px;
  }
  .nodata p {
    font-size: 14px;
    color: #999;
    margin: 10px 0 0 10px;
    line-height: 100%;
  }
  .selectBoxFull {
    position: relative;
    width: 100%;
    padding-right: 0px;
    box-sizing: border-box;
    height: 34px;
    margin: 10px 0;
  }
  .withdrawal input[type="text"] {
    background: #f3f3f3;
    display: inline-block;
    width: 100%;
    border: 1px solid #e6e6e6;
    box-sizing: border-box;
    padding: 0 10px;
    line-height: 32px;
    font-size: 14px;
  }
  .submitBtn .btn-block {
    font-size: 14px;
    width: 100%;
    background: #5b8fef !important;
    color: #c8d8fc!important;
    border: none;
    height: 34px;
    text-align: center;
    padding: 9px 0;
    cursor: pointer;
    font-weight: 400;
    border-radius: 0.25rem;
  }
  .btn-orange:hover {
    background: #317aff!important;
    border-color: #f1c55f!important;
    color: #fff!important;
  }
  .total-content span {
    padding: 5px 60px;
  }
  .tips {
    margin-top: 20px;
  }
  .tips strong {
    color: #333;
    font-size: 12px;
    display: inline-block;
    margin-bottom: 6px;
    font-weight: bold;
  }
  .tips p {
    font-size: 12px;
    color: #666;
    line-height: 20px;
  }
</style>
