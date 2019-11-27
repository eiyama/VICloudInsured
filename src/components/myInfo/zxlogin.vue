<template>
  <div :style="bodyBox" class="login">
   <div class="mint-content mint-region">
        <mt-field class="mint-formItm" :disabled="true">
          <img :src="phoneImg" style="width:20px;">
          <input class="mint-input" v-model="phone" type="number" maxlength='11' placeholder="请输入手机号">
        </mt-field>
        <mt-field class="mint-formItm" style="position: relative;" :disabled="true">
          <img :src="pswImg" style="width:20px;">
          <input class="mint-input" v-model="verifyCode" placeholder="请输入验证码">
             <div style="position: absolute;right: 0;bottom:2px">
                 <p class="btnTitle" :style="verifyBtn" @click="getVerifyCode" >{{btnTitle}}</p>
            </div> 
        </mt-field> 
        <div style="padding:0px 10px">
            <mt-button class="btn-login" type="primary" @click="jumpreg" size="large">登录</mt-button>
        </div>
        <p style="color:#4A66FE;text-align: center; margin: 30px;" @click="toRegister">我要注册</p>
        <!-- <div class="footer-banner">
        </div> -->
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
  export default {
    data() {
      return {
        bodyBox: {
          'minHeight': '',
          'background': '#fff',
          'padding': '40px 0'
        },
        appId: '',
        nickName: '',
        title: '',
        referrerMobile: '',
        shareUrl:'',
        phoneImg: (this.src = require("../../assets/images/phone-icon.png")),
        pswImg: (this.src = require("../../assets/images/verify-icon.png")),
        canReg: false,
        urlFrom: "",
        code: "",
        enableLogin: false,
        openId: "",
        btnTitle:'获取验证码',
        verifyBtn:{
            "backgroundColor":'#fff',
            "color":"#4A66FE"
        },
        countTime:'60',
        disabled:false,
        phone:'',
        verifyCode:'',
        realName:'',
        zeRegister:{"backgroundColor":"#FFF","minHeight": "650px" },
        InsuranceCard:[],
        popOrganizationVisible: false,
        organization: '',
        organizationVal: '',
      }
    },
    created: function() {
      var h = window.innerHeight;
      this.bodyBox.minHeight = h - 100 + 'px';
    },
    mounted() {
      this.appId = this.$route.query.appId;
      localStorage.setItem("appId", this.appId);
    },
    methods: {
       searchOrg(e){
        if(this.InvitationCode){
             axiosGetAPI('/Users?userCode=' + this.InvitationCode).then((res) => {
                if (res.status == 200 && res.result) {
                this.hasOrgResult = true;
                this.organizationVal = res.result.orgName;
                this.realName = res.result.realName;
                this.customerData.ReferrerMobile = res.result.userName;
                this.customerData.OrgCode = res.result.orgCode;
                this.organization = res.result.orgName;
                }
                else{
                     this.hasOrgResult = false;
                     this.organizationVal = '';
                }
            });
        }else{
            this.hasOrgResult = false;
            return;   
        }
      },
   getVerifyCode(){
        var self = this;
         if(!this.phone){
          Toast('请输入手机号');
          return;  
        }
        if(this.phone){
            if(!(/^1[34578]\d{9}$/.test(this.phone))){ 
            Toast("手机号码有误，请重新输入");  
            return false; 
            } 
        }
        // if(!this.openId){
        //   Toast('未获取微信登录权限，请先授权微信');
        //   return;  
        // }
        if(this.countTime < 60){
            return;
        }
         if(this.phone){
          //  axiosGetAPI('/Users?userName=' + this.phone).then((res) => {
          //     if(res.result){
          //         this.verifyBtn.backgroundColor = "#f0f0f0";
          //         this.verifyBtn.color = "#999";
          //   axiosGetAPI('/Weixin/'+ this.openId + '/captcha?phone=' + this.phone).then(res=>{
          //       if(res.status == "200"){
          //           let timer = setInterval(() => {
          //               if( this.countTime == 0) {
          //                   clearInterval(timer);
          //                   this.countTime = 60;
          //                   this.disabled = false;
          //                   self.verifyBtn.backgroundColor = "#4A66FE";
          //                   self.verifyBtn.color = "#fff";
          //                   this.btnTitle = "获取验证码";
          //               } else {
          //                   this.btnTitle = this.countTime + '秒后重试';
          //                   this.disabled = true;
          //                   this.countTime--
          //               }
          //               },1000)
          //           }    
          //       })
          //     }else{
          //        Toast('该手机号已注册，请重新输入');
          //        return;
          //     }
          // })  
        }
    },
    jumpreg(){

    },
    toRegister(){
      this.$router.push({path:'/myInfo/zxregister'})
    }
    }
  }
</script>
<style scoped>
  
.login .mint-formItm .mint-input {
  width: 85%;
  margin-right: 0;
  color: #333;
  font-size: 14px;
}

.login .btn-login {
  margin-top: 50px;
  background:#4A66FE;
}

.login .fn-clear-j {
  color: #888991;
  padding: 10px;
}

.login .fn-clear-j p {
  float: right;
}

.login .fn-clear-j a {
  color: #fff
}
.login-images img{
  width: 100%;
  height: 160px;
  margin-bottom: 15px;
}

.btnTitle{
    font-size: 12px;
    color:#999;
    background: #f0f0f0;
    padding: 0 10px;
    border-radius: 6px;
    height: 32px;
    line-height: 32px;
    border:1px solid #eeeeee;
}

.btnInvitation{
    position:absolute;
    bottom: 0;
    padding: 10px;
    font-size: 14px;
    color:#999;
    background: #f0f0f0;
    right: 0;
    /* padding: 0 10px; */
}

.tip{
    padding-top: 20px;
    line-height: 40px;
    /* padding: 1px 0; */
    margin: 0 15px;
    /* border-bottom: 1px solid #e0e0e0; */
}

.tip1{
    padding: 10px 0;
    margin: 0 15px;
    border-top: 1px solid #e0e0e0;
    border-bottom: 1px solid #e0e0e0;  
}

.footer-banner{
    background: url('./../../assets/images/regiter-banner.png')no-repeat 0 0;
    background-size: cover;
    height: 150px;
    opacity: 0.6;
    position: absolute;
    bottom: 0;
    width: 100%;
    z-index: 5;
}
</style>
