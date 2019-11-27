<template>
  <div style="background:#F5F5F5;">
    <header class="header_box" style="display:block;">
      <div class="header">
        <div @click="go_back" class="header_750 cb">
          <a class="go_back" href="javascript:void(0)">
            <span class="base_icon"></span>
          </a>
          <h2>支付订单</h2>
        </div>
      </div>
    </header>
    <div class="mainBox">
      <h3>订单金额(元)</h3>
      <h4 class="account">
        <span>￥</span>
        <p>{{amount}}</p>
      </h4>
      <div class="productName">
        <p>商品名称</p>
        <p>房咚保({{insuredName == 'PICC' ? '中国人保' : '太平洋保险'}})出租屋责任险-{{productType}}</p>
      </div>
    </div>
    <div style="background:#fff">
      <div class="insuranceColumnTop" style="border-top: .17rem solid #F5F5F5;">
        <h3 class="newCTitleBox_2">请选择支付方式</h3>
      </div>
    </div>
    <div class="payWay">
        <i><img src="../../assets/images/insurance_2/duigongzhanghu_icon.png" alt=""></i>
        <div class="list">
            <p>保险公司对公转账</p>
        </div>
        <i style=" position: absolute;right: 16px;"><img style="height:13px" src="../../assets/images/chevron.png" alt=""></i>
    </div>
    <div class="payBtn">
       <p>去支付</p> 
    </div>
  </div>
</template>
  <script >
import "./../../assets/css/zxfd/base.scss";
// import "./../../assets/js/rem.js";
// import fwczrList from "./fwczrList.vue";
// import jjzrList from "./jjzrList.vue";
// import jtcczhList from "./jtcczhList.vue";
// import sanzexianList from "./sanzexianList.vue";
const typeList = ['标准版','豪华版','尊享无忧版','温馨版'];

export default {
  data() {
      return{
        amount:'',
        productType:'',
        insuredName:''
      }
  },
  methods: {
    	go_back(){
				this.$router.back();
			},

  },
  created() {
    var nonCarList = JSON.parse(sessionStorage.getItem('nonCarList'));
    var fdInsuranceData = JSON.parse(sessionStorage.getItem('fdInsuranceData'));
    this.amount = nonCarList.amount;
    this.productType = typeList[fdInsuranceData.packageType];
    this.insuredName = nonCarList.orderDetail.InsCorpCode;
  },
  mounted() {},
};
</script>
<style scoped>
.base_msg_box > div > a.sbtn {
  color: #333;
  background: linear-gradient(
    90deg,
    rgba(255, 226, 59, 1) 0%,
    rgba(255, 213, 0, 1) 100%
  );
  box-shadow: 0px 2px 5px 0px rgba(255, 192, 0, 0.7);
  border-radius: 0.8rem;
  width: 80%;
  margin: 0.3rem auto;
}
.base_msg_box > h3 {
  padding-top: 0.45rem;
  font-size: 0.36rem;
}
.insuranceSetBox,
.insuranceTextBottom {
  padding-right: 0.3rem;
}
.insuranceTextBottom {
  border: none;
}
.mainBox {
  height: 158px;
  background: rgba(255, 255, 255, 1);
  padding-top: 1px;
}
.mainBox h3 {
  font-size: 16px;
  color: rgba(153, 153, 153, 1);
  text-align: center;
  margin-top: 23px;
}
.mainBox .account {
  display: flex;
  justify-content: center;
  align-items: center;
}
.mainBox .account span {
  font-size: 20px;
  color: rgba(255, 18, 18, 1);
  margin-top: 8px;
  font-weight: 400;
}
.mainBox .account p {
  font-size: 32px;
  font-weight: 500;
  color: rgba(255, 18, 18, 1);
}
.productName {
  margin-top: 24px;
  padding: 0 16px;
  display: flex;
  align-items: center;
}
.productName p:nth-child(1) {
  margin-right: 30px;
  font-size: 14px;
  color: rgba(153, 153, 153, 1);
}
.productName p:nth-child(2) {
  font-size: 14px;
  color: rgba(51, 51, 51, 1);
}
.payWay {
  /* border-top: 10px solid rgba(245, 245, 245, 1); */
    display: flex;
    background: #fff;
    align-items: center;
    padding: 0 16px;
    height: 52px;
}


.payWay .list p {
  font-size: 16px;
  color: rgba(51, 51, 51, 1);
    font-size:16px;
    font-family:PingFangSC-Regular,PingFang SC;
    font-weight:400;
    margin-left: 16px;
}

.payBtn{
    margin: 24px 16px;
    height:44px;
    background:linear-gradient(90deg,rgba(255,226,59,1) 0%,rgba(255,213,0,1) 100%);
    box-shadow:0px 2px 5px 0px rgba(255,192,0,0.7);
    border-radius:22px;  
}

.payBtn{
    text-align: center;
    line-height: 44px;
    font-size:15px;
    font-weight:400;
    color:rgba(51,51,51,1);

}

</style>