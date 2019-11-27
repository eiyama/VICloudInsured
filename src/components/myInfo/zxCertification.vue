<template>
  <div class="home" style="background-color:#F5F5F5">
    <mt-header fixed class="def-header cust-head" title="个人实名认证">
      <div slot="left" @click="openBack">
        <van-icon name="cross" size="20px" />
        <!-- <img :src="closeImg" style="width: 18px;height: 18px;" > -->
      </div>
    </mt-header>
    <iframe :src="toCertificationcpage" frameborder="0" width="100%" :height=isHeight></iframe>
  </div>
</template>
<script>
import { axiosGetAPI, axiosPostAPI } from './../../common/Axios';
import { Toast, Indicator } from 'mint-ui';
export default {
  data() {
    return {
     userName:'',
     isHeight:'',
      closeImg: (this.src = require("../../assets/images/ico-close.png")), 
      toCertificationcpage:'',
      salesmanId:''
    }
  },
  created: function() {
   this.isHeight = window.innerHeight  + 'px';
   this.salesmanId = sessionStorage.getItem('salesmanId');
    //  var currentUrl = window.location.href;
     var url = window.location.protocol + '//' + window.location.host + '/#' + '/myInfo/myIndex?appId=' + localStorage.getItem('appId');
    var callback = url;
    // var callback = "''"
    // console.log(callback)
  Indicator.open('请稍后...')
   axiosGetAPI('/Salesmans/'+ this.salesmanId + '/Authentication?callback=' + callback).then(res=>{
     Indicator.close()
     if(res.status){
        this.toCertificationcpage = res.result;
     }
   })
  },
  mounted() {

      
  },
  methods: {
    openBack: function() {
      // window.history.back();
      this.$router.push({path: '/myInfo/myIndex'});
    },
  }
}

</script>
<style scoped>
.cardBox{
    height: 300px;
    background: #f0f0f0;
    padding: 20px;
}
.addidCard{
  margin: 20px;
  background: #fff;
  padding: 10px 0;
  display: flex;
  justify-content: center;
  align-items: center;
  height: 130px;
  border-radius: 8px;
}
.uploadIdcard{
    height: 250px;
    background: #4A66FE;
    padding-top: 0.5px;
    border-radius: 8px;
}


</style>
