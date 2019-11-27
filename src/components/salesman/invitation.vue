<template>
  <div :style="bodyBox">
    <div style="display: flex;justify-content: center;"><img style="height: 40%;width:40%" src="../../assets/images/img-joinUs.png" />
    </div>
    <div style="display: flex;justify-content: center;align-items: center;margin-top: 30px;">
      <img style="height: 42px;" src="../../assets/images/icon-avatar.png" />
      <p style="font-size: 24px;color: #FFFFFF;margin-left: 15px;">{{nickName}}</p>
    </div>
    <div class="recommend">
      <p >我已在{{title}}入驻，这里保险展业体验真不错，你也一起加入吧~</p>
    </div>

    <div class="goReg"> 
        <span  class="joinBtn" @click="goReg">立即加入</span>
    </div>
    
    <!-- <div class="scanCode">
      
      <p v-show="iszxPage" style="padding:10px 0;color:#f44">长按识别二维码加入</p>
      <img v-show="iszxPage" style="width: 150px;height:150px;" :src="scanCode" alt="">
    </div> -->
    <div class="footerBox">
      <img style="width:100%" src="../../assets/images/img-txt-001.png" />
    </div>

  </div>
</template>
<script>
  import {
    axiosGetAPI,
    axiosPostAPI
  } from './../../common/Axios';
  import {
    Indicator,
    Toast
  } from 'mint-ui';
  // import QRCode from 'qrcodejs2';
  import router from './../../router';
  // import echarts from "echarts";
  import store from './../../store/store';
  export default {
    data() {
      return {
        bodyBox: {
          'minHeight': '',
          'background': '#2965f9',
          'padding': '40px 0',
        },
        appId: '',
        nickName: '',
        title: '',
        referrerMobile: '',
        shareUrl:'',
        iszxPage:false,
        scanCode:'',
        userCode:''
      }
    },
    created: function() {
      var h = window.innerHeight;
      this.bodyBox.minHeight = h - 100 + 'px';
    },
    mounted() {
      this.iszxPage = this.$route.query.iszxPage;
      console.log(this.iszxPage);
      this.appId = this.$route.query.appId;
      // this.iszxPage = (this.appId == 'cx7190761141531rzi' || this.appId == 'cxa181211170855pxj');
      // if(this.iszxPage){
      //    axiosGetAPI("/Weixin/QRCode").then((res) => {
      //     if (res.status == 200 ) {
      //       this.scanCode = res.result;
      //       }
      //     })
      // }
      localStorage.setItem("appId", this.appId);
      this.nickName = decodeURIComponent(this.$route.query.nickName);
      // this.referralCode = this.$route.query.referralCode;
      this.title = decodeURIComponent(this.$route.query.title);
      // this.userId = this.$route.query.userId;
      this.referrerMobile = this.$route.query.ReferrerMobile;
      this.userCode = this.$route.query.userCode;
      // var domain =  document.location.toString().split("#")[0];
      //  this.shareUrl = domain + '#/regUser?appId=' + this.appId +
      // '&nickName=' + this.nickName +
      // // '&referralCode=' + this.referralCode +
      // '&title=' + this.title +
      // // '&UserId=' + this.userId +
      // '&ReferrerMobile=' + this.referrerMobile +
      // '&isShare=true';
       this.$nextTick(function() {
      // this.creatQrCode();
    })
    },
    methods: {
      goReg:function(){
          if(this.appId == 'cx7190761141531rzi' || this.appId == 'cxa181211170855pxj'){
               var param = '?appId=' + this.appId +'&nickName=' + this.nickName +'&title=' + this.title +
              '&ReferrerMobile=' + this.referrerMobile +'&isShare=true' + '&userCode=' + this.userCode;
              this.$router.push({ path: "/myInfo/zxregister" + param});
          }else{
              var param = '?appId=' + this.appId +'&nickName=' + this.nickName +'&title=' + this.title +
              '&ReferrerMobile=' + this.referrerMobile +'&isShare=true';
               this.$router.push({ path: "/regUser" + param});
          }
      },
    }
  }
</script>
<style scoped>
  .recommend {
    background: url("../../assets/images/bg-pop-dashline.png")no-repeat center;
    height: 100px;
    background-size: 80%;
    display: flex;
    justify-content: center;
    align-items: center;
  }

  .recommend p {
    width: 260px;
    color: #fcf8f0;
    font-size: 14px;
  }

  .joinBtn {
    background: #fec22c;
    color: #FFFFFF;
    margin: 30px 40px;
    text-align: center;
    padding: 10px 0;
    display: block;
    border-radius: 8px;
    font-size: 16px;
  }

  .footerBox{
    position: fixed;
    bottom: 0;
    left: 0;
    right: 0;
    height: 160px;
  }

  .qr-pos {
  margin-top: -10px;
  display: inline-block;
}

.scanCode{
    text-align: center;
    /* margin-top: 20px; */
    max-width: 250px;
    margin: auto;
    background: #f2f2f2;
    margin-top: 20px;
    border-radius: 5px;
    padding-bottom: 10px;
}
.goReg{
    max-width: 250px;
    margin: auto;
}

</style>
