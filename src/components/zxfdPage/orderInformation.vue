<template>
	<div style="background:#F5F5F5;">
        <header class="header_box" style="display:block;">
        	<div class="header">
            	<div class="header_750 cb">
                	<a @click="go_back" class="go_back" href="javascript:void(0)">
                    	<span class="base_icon"></span>
                    </a>
                    <h2>订单信息</h2>
                </div>
            </div>
        </header>
        <div class="insuranceColumn" style="border-top:0px;">
        	<div class="insuranceColumnTop">
            	<h3 class="newCTitleBox_2">投保人信息<span class="newCTitle_explain">(保单购买者)</span></h3>
            </div>
             <ul class="messageList">
             	<li class="cb pr">投保人 <i>*</i>
                    <input  type="text" class="insuranceIpt_2" placeholder="请输入姓名" v-model="upData.owner.InsuredName">
            	</li>
                <li class="cb pr">证件类型 <i>*</i>
                	<input name="applyIdType" type="hidden" value="01" class="setIptValue" readonly>  
                    <input name="" type="text" class="insuranceIpt" placeholder="请选择"  v-model="upData.owner.IdentifyTypeName" readonly @click="hasidType = true">
            	</li>
                <li class="cb pr">投保人证件号 <i>*</i>
                    <input name="applyIdNumber" type="text" class="insuranceIpt_2" placeholder="请输入证件号"  v-model="upData.owner.IdentifyNumber" >
            	</li>
                <li class="cb pr">投保人手机 <i>*</i>
                    <input name="applyMobile" type="number" oninput="if(value.length>11)value=value.slice(0,11)" class="insuranceIpt_2" placeholder="请输入手机"  v-model="upData.owner.PhoneNumber">
            	</li>
                <li class="cb pr">投保人邮箱
                    <input name="applyEmail" type="text" class="insuranceIpt_2" placeholder="用于接收电子版保单"  v-model="upData.owner.email" >
            	</li>
                
             </ul>
        </div>
        <div class="insuranceColumn">
            <div class="insuranceColumnTop">
            	<h3 class="newCTitleBox_2" >被保险人资料<span class="newCTitle_explain">(保障对象，赔款接收人)</span></h3>
            </div>
            <ul class="messageList">
            	<li class="cb pr">是否为投保人本人
                    <p class="slideBtn"  ><label for="check_1"  @click.stop="slideSwitch"><span :class="{current : sameOwner == true}"><!--current--><i></i></span></label></p>
            	</li>
            </ul>
            <ul class="messageList" v-show="!sameOwner">     
                <li class="cb pr">被保险人姓名 <i>*</i>
                    <input type="text" class="insuranceIpt_2" placeholder="请输入姓名"   v-model="upData.applicant.InsuredName" >
            	</li>
				<li class="cb pr">证件类型 <i>*</i>
                	<input name="applyIdType" type="hidden" value="01" class="setIptValue" readonly>  
                    <input name="" type="text" class="insuranceIpt" placeholder="请选择"  v-model="upData.applicant.IdentifyTypeName" readonly @click="hasApplicant = true">
            	</li>
                <li class="cb pr">被保险人证件号 <i>*</i>
                    <input name="insuredIdNumber" type="text" class="insuranceIpt_2"  v-model="upData.applicant.IdentifyNumber" placeholder="请输入证件号" >
            	</li>
                <li class="cb pr">被保险人手机号 <i>*</i>
                    <input name="insuredMobile" type="text" class="insuranceIpt_2"  v-model="upData.applicant.PhoneNumber" placeholder="请输入手机" >
            	</li>
            </ul>
        </div>
        <div style="background:#fff">
            <div class="insuranceColumnTop" style="border-top: .17rem solid #F5F5F5;">
            	<h3 class="newCTitleBox_2" >承保单位</h3>
            </div>
            <div style="padding:.3rem; padding-bottom:.1rem;">
            	<div class="nweInsuranceSetBox newWidth">
                    <a :class="{current: InsuranceCode == 'PICC'}" @click="setRadioIpt('PICC')"><input name="company" type="radio" value="PICC" id="sRadio_1"><label for="sRadio_1"><img style="height: 0.24rem;" src="../../assets/images/insurance/renbao.png" alt=""> 中国人保<br><i>（支持全国投保）</i></label></a>
                    <a :class="{current: InsuranceCode == 'CPIC'}" @click="setRadioIpt('CPIC')"><input name="company" type="radio" value="CPIC" id="sRadio_2"><label for="sRadio_2"><img style="height: 0.32rem;" src="../../assets/images/insurance/taibao.png" alt=""> 太平洋保险<br><i>（暂只支持深圳投保）</i></label></a>
               </div>
            </div>
            <div class="insuranceColumnTop" style="border-top: .17rem solid #F5F5F5;">
            	<h3 class="newCTitleBox_2" >订单信息</h3>
            </div>
            <ul class="messageList">
                <li class="cb pr js_address_c1">投保房产地址
                	<input name="address_1" type="hidden" value="" class="setIptValue" id="value1" readonly>  
                    <input name="address_text" type="text" :value="cityValue" class="insuranceIpt" placeholder="请选择省市区" id="demo1" @click="toAddress" readonly>
            	</li>

                <li class="cb pr js_address_c2" >
  					<input name="address_2" type="text" class="insuranceIpt_2" placeholder="请输入楼栋详细地址" v-model="upDataList.orderDetail.DetailedAddress" >
            	</li>
                
            	<li class="cb pr">投保部分房间
                    <p class="slideBtn"  ><label for="check_1"  @click.stop="someHouse"><span :class="{current : issomeHouse == true}"><!--current--><i></i></span></label></p>
            	</li>
            
             	<li class="cb pr">
                    <input style="width:100%;" name="address_3" type="text" v-model="detailRoom"  class="insuranceIpt_2" placeholder="请输入房间号，以“,”分隔" >
            	</li>
             </ul>
             <ul class="annotation">
                 <li>1、本保单仅承保住宅房屋，经营性质场所不在保障范围内。</li>
                 <li>2、如投保人投保多个地址，须单独进行投保，不可合计面积进行选择，否则本保单不承担保险责任。</li>
             </ul>
             <ul class="messageList">
               <li class="cb pr" style="border-top: 1px solid #eee">生效日期<i>*</i>
                	<input name="startDate" type="hidden" value="" class="setIptValue" readonly>  
                    <input name="" type="text" class="insuranceIpt" v-model="CommencementDateValue" placeholder="请选择" readonly @click="hasCommencementDate = true" >
            	</li>
            </ul>
              <p class="dateAnnotation">将于下个工作日结束前完成审核出单，正式保险生效日期可能因出单时间顺延</p>
        </div>
        <div style="background:#fff;">
            
          <ul class="messageList" >
                <li class="cb pr js_address_c1" style="border-top: 0.01rem solid #F5F5F5;" >保险期限
                	<input name="address_1" type="text" :value="qx" class="insuranceIpt_2" readonly>  
            	</li>
                <li class="cb pr js_address_c1" >投保方式
                	<input name="address_1" type="text" :value="insuranceMethod" class="insuranceIpt_2" readonly>  
            	</li>
                <li class="cb pr js_address_c1" >选择套餐
                	<input name="address_1" type="text" :value="setMeal" class="insuranceIpt_2" readonly>  
            	</li>
                <li class="cb pr js_address_c1" >投保面积(建筑面积)
                	<input name="address_1" type="text" :value="area" class="insuranceIpt_2" readonly>  
            	</li>
                <li class="cb pr js_address_c1" >电梯责任保障
                	<input name="address_1" type="text" :value="elevatorInsurance + '份'" class="insuranceIpt_2" readonly>  
            	</li>
                <li class="cb pr js_address_c1" >租金损失保障
                	<input name="address_1" type="text" :value="hireLossInsurance + '份'" class="insuranceIpt_2" readonly>  
            	</li>
                <li class="cb pr js_address_c1" >租房声誉损失津贴保障
                	<input name="address_1" type="text" :value="reputationInsureance + '份'" class="insuranceIpt_2" readonly>  
            	</li>
             </ul>  
              <h4 style="margin-top:0.34rem;margin-left:0.32rem;font-weight:400;">
                	<span style="color: rgba(51,51,51,1);font-size: 0.3rem;">保障内容</span>
                </h4> 
              <div style="margin: 0.32rem;">
                	<table class="nweInsuranceTab">
					<tr >
						<th style="border: 0;">基本保障内容</th>
						<th>赔偿限额</th>
						<th>保障</th>
					</tr>
                	<tr>
                    	<td rowspan="5">第三者(含租客)人身伤亡及财产损失赔偿责任(含水管爆裂责任、高空抛物责任)  </td>
                        <td>累计责任限额</td>
                        <td class="">{{premiumList[0]}}万</td>
                    </tr>
                    <tr>
                    	<td>每次事故责任限额</td>
                        <td class="">{{premiumList[1]}}万</td>
                    </tr>
                    <tr>
                    	<td>每次事故每人伤亡责任限额</td>
                        <td class="">{{premiumList[2]}}万</td>
                    </tr>
                    <tr>
                    	<td>每次事故财产损失责任限额</td>
                        <td class="">{{premiumList[2]}}万</td>
                    </tr>
                    <tr>
                    	<td>法律费用累计责任限额</td>
                        <td class="">{{premiumList[4]}}万</td>
                    </tr>
                    <tr>
                    	<td rowspan="4">出租人家庭财产<br>损失赔偿责任</td>
                        <td>房屋主体、附属建筑物</td>
                        <td class="">{{premiumList[5]}}万</td>
                    </tr>
                    <tr>
                    	<td>室内附属设施</td>
                        <td class="">{{premiumList[6]}}万</td>
                    </tr>
                    <tr>
                    	<td>室内装潢</td>
                        <td class="">{{premiumList[7]}}万</td>
                    </tr>
                    <tr>
                    	<td>室内财产</td>
                        <td class="">{{premiumList[8]}}万</td>
                    </tr>
                    <tr>
                    	<td>电梯第三者人身伤害赔偿责任</td>
                    	<td>每次事故每人伤亡责任限额</td>
                        <td class="">{{extendPremium[0] > 0 ? extendPremium[0] + "万" : extendPremium[0] }}</td>
                    </tr>
                    <tr>
                    	<td>租金损失赔偿责任</td>
                    	<td>最高赔偿限额</td>
                        <td class="">{{extendPremium[1] > 0 ? extendPremium[1] + "万" : extendPremium[1] }}</td>
                    </tr>
                    <tr>
                    	<td>租房声誉损失津贴</td>
                    	<td>每次事故赔偿限额</td>
                        <td class="">{{extendPremium[2] > 0 ? extendPremium[2] + "万" : extendPremium[2] }}</td>
                    </tr>
                </table>
                </div>  
        </div>
        <div style="height:1.5rem;">
        	<div class="insuranceBttomBtnBox">
                <div class="cb">
					<div >
						<span><b style="color:#333; font-weight:500;">保险费用: </b><i class="totalAmount"> {{account}}</i> 元</span>
						<a style="color:#0080FF" href="javascript:void(0)" @click="showInsured('text')">《保险条款》</a>
					</div>
                    <a style="margin-top: 15px;" class="insuranceBttomBtn" href="javascript:void(0)" @click="submitInsurance">确认支付</a>
                </div>
            </div>
        </div>   
		<div class="black_box" style="display:none;">
            <div class="base_msg_box">
            	<a class="closedBtn" @click="cancelEvent()"><span></span></a>
            	<h3>请仔细阅读以下协议</h3>
            	<div class="base_msg_text" v-if="InsuranceCode == 'PICC'">
                	<a @click="showFwczrList = true" style="font-weight:500;">《房屋出租人责任险条款》</a>
                    <a @click="showDszssList = true" style="font-weight:500;">《附加第三者损失保险条款》</a>
                    <a @click="showJtcczhList = true" style="font-weight:500;">《附加家庭财产综合保险条款》</a>
                    <a @click="showSpecialAgreement = true" style="font-weight:500;">《特别约定》</a>
					<a @click="showInsuranceDec = true" style="font-weight:500;">《投保声明》</a>
                </div>
				<!-- <div class="base_msg_text" v-if="InsuranceCode == 'CPIC'">
                	<a @click="showFwczrList = true" style="font-weight:500;">《出租人责任保险条款》</a>
                    <a @click="showDszssList = true" style="font-weight:500;">《出租人责任保险附加第三者责任保险条款》</a>
                    <a @click="showJtcczhList = true" style="font-weight:500;">《出租人责任保险附加出租人财产损失保险条款》</a>
                    <a @click="showSpecialAgreement = true" style="font-weight:500;">《出租人责任保险附加房屋租金损失保险条款》</a>
					<a @click="showInsuranceDec = true" style="font-weight:500;">《出租人责任保险附加出租房声誉损失津贴保险条款》</a>
					<a @click="showInsuranceDec = true" style="font-weight:500;">《特别约定》</a>
                </div> -->
            	<div class="cb"><a class="sbtn" href="javascript:void(0)" @click="insuredOperate">同意协议并投保</a></div>
            </div>
        </div>

		<van-popup v-model="hasApplicant" position="bottom">
			<van-picker
				show-toolbar
				:columns="typeResult"
				@cancel="hasApplicant = false"
				@confirm="onApplicantConfirm"
			/>
		</van-popup>

		<van-popup v-model="hasidType" position="bottom">
			<van-picker
				show-toolbar
				:columns="typeResult"
				@cancel="hasidType = false"
				@confirm="onidTypeConfirm"
			/>
		</van-popup>

		<van-popup v-model="hasCommencementDate" position="bottom">
			<van-datetime-picker
				show-toolbar
				type="date"
				v-model="commencementDate"
				@cancel="hasCommencementDate = false"
				@confirm="onCommencementDate"
			/>
		</van-popup>

		<van-popup v-model="mask" position="bottom" style="height:45%">
			<div class="headerClose" @click="closeCitySelect" ><van-icon size="18px" name="cross" style="float:right;margin-top: 10px;margin-right:10px;" /></div>
			<v-distpicker type="mobile" style="margin-top:30px" @selected='selected' v-show="hasCity">
			</v-distpicker>
			<div class="mask" v-show="mask"></div>
		</van-popup>
		
		<van-popup v-model="showFwczrList" round style="height:80%" position="bottom">
			<a class="closedPopupBtn" @click="showFwczrList = false"><span></span></a>
			<fwczrList :title="transmissionTitle"></fwczrList> 
		</van-popup>
		
		<van-popup v-model="showDszssList"  round style="height:80%" position="bottom">
			<a class="closedPopupBtn" @click="showDszssList = false"><span></span></a>
			<sanzexianList :title="transmissionTitle"></sanzexianList> 
		</van-popup>

		<van-popup v-model="showJtcczhList"  round style="height:80%" position="bottom">
			<a class="closedPopupBtn" @click="showJtcczhList = false"><span></span></a>
			<jtcczhList :title="transmissionTitle"></jtcczhList> 
		</van-popup>

		<van-popup v-model="showSpecialAgreement"  round style="height:80%" position="bottom">
			<div class="closedPopupBtn" @click="showSpecialAgreement = false"><span></span></div>
			<specialAgreement :title="transmissionTitle"></specialAgreement> 
		</van-popup>

		<van-popup v-model="showInsuranceDec"  round style="height:80%" position="bottom">
			<div class="closedPopupBtn" @click="showInsuranceDec = false"><span></span></div>
			<InsuranceDec :title="transmissionTitle"></InsuranceDec> 
		</van-popup>

    </div>
</template>
  <script >
   import "./../../assets/css/zxfd/base.scss";
   import city from "./../../assets/json/city.json";
   import fwczrList from "./fwczrList.vue";
   import jjzrList from "./jjzrList.vue";
   import jtcczhList from "./jtcczhList.vue";
   import sanzexianList from "./sanzexianList.vue"; 
   import specialAgreement from "./specialAgreement.vue";   
   import InsuranceDec from "./InsuranceDec.vue";
   import VDistpicker from 'v-distpicker'; //省市区选择器
   import { Toast ,Indicator } from 'mint-ui';
import { axiosGetAPI, axiosPostAPI } from '../../common/Axios';
   const typeList = ['标准版','豪华版','尊享无忧版','温馨版'];
   const setMealCode = ['FA001','FA002','FA003','FA004','FA005'];// 套餐code
   const insuranceMethodList = ['按栋投保','按户投保'];
   const areaCodeList = ['BF001','BF002','BF003','BF004'];
	//    房东版=FDD001，租客版=FDZ001
 export default {
    data(){
        return{
			qx:"一年",
            premiumList:[300,300,150,20,5,50,10,10,10],
            extendPremium:[0,0,0],
            sameOwner:false,
            upData:{
                owner: { InsuredName: "", PhoneNumber: "", IdentifyNumber: "", IdentifyType: "", InsuredAddress:"",email:"",IdentifyTypeName:""},
                applicant: { InsuredName: "", PhoneNumber: "", IdentifyNumber: "", IdentifyType: "", InsuredAddress:"",IdentifyTypeName:''},
			},	
			upDataList:{
					"distributorCode": "QD0000001",
					"productCode": "FDD001", //string,产品编号
					"startDate": "", //date-time,起保日期
					"endDate": "", //date-time,终保日期
					"amount": "", //double,订单金额
					"applicantName": "", //string,投保人
					"insurantName": "", //string,被保人
					"orderDetail": {
						"InsCorpCode": "",
						"AreaCode": "",
						"DetailedAddress": "",
						"ProductType": "",
						"SchemeCode": "",
						"PremiumCode": "",
						"Extendeds": {},
						"RelationPeoples": [],
					},
					"distributorAttach": ""
			},
			InsuranceCode:"PICC",              
			typeResult : ['中华人民共和国身份证', '港澳居民来往内地通行证', '台胞证','士兵证/军官证/警官证','中国护照','港澳身份证','外国人永久居留身份证','机构代码','统一社会信用代码','其他'],
			typeCode:[{name:'中华人民共和国身份证',code:'IdCard'},{name:'港澳居民来往内地通行证',code:'MacHKerPassport'},{name:'台胞证',code:'TaiwanesePassport'},{name:'士兵证/军官证/警官证',code:'Soldier'},{name:'中国护照',code:'ChinaPassPort'},
			{name:'港澳身份证',code:'GAIdCard'},{name:'外国人永久居留身份证',code:'ForeignForeverIdCard'},{name:'机构代码',code:'Organization'},{name:'统一社会信用代码',code:'CreditCode'},{name:'其他',code:'Other'}],
			hasidType:false,
			hasCommencementDate:false,
			CommencementDateValue:'',
			commencementDate:new Date(this.Common.Public.GetDateStr(0)),
			hasCity:false,
			mask:false,
			cityList:[],
			falg:true,
			textFalg:false,
			showFwczrList:false,
			showDszssList:false,
			showJtcczhList:false,
			showJjzrList:false,
			showSpecialAgreement:false,
			showInsuranceDec:false,
			account:'',
			area:'',
			packageType:'',
			areaValue:'',
			elevatorInsurance:'',
			hireLossInsurance:'',
			reputationInsureance:'',
			insuranceMethod:'',
			setMeal:'',
			transmissionTitle:'',
			cityValue:"",
			hasApplicant:false,
			issomeHouse:false,
			detailRoom:'',
			// onApplicantConfirm:false,
			// 标准版方案(房东版)，居家责任保险(租客版)	FA001
			// 豪华版方案(房东版)，电动自行车第三者责任险(租客版)	FA002
			// 尊享无忧版(房东版)，监护人责任险(租客版)	FA003
			// 温馨版方案(房东版)，燃气责任险(租客版)	FA004
			// 宠物责任险(租客版)	FA005

			// 			房东版
			// 500㎡-1000㎡（按栋投），1㎡-50㎡（按户投）	BF001
			// 1001㎡-2000㎡（按栋投），51㎡-90㎡（按户投）	BF002
			// 2001㎡-3000㎡（按栋投），91㎡-130㎡（按户投）	BF003
			// 3001㎡-5000㎡（按栋投），3001㎡-5500㎡（按栋投），131㎡-200㎡（按户投）	BF004

			// 租客版
			// 1㎡-35㎡(居家责任)，电动自行车第三者责任险，监护人责任险，燃气责任险-瓶装，宠物责任险	3个月	BF001
			// 1㎡-35㎡(居家责任)，电动自行车第三者责任险，监护人责任险，燃气责任险-瓶装，宠物责任险	6个月	BF002
			// 1㎡-35㎡(居家责任)，电动自行车第三者责任险，监护人责任险，燃气责任险-瓶装，宠物责任险	12个月	BF003
			// 36㎡-80㎡(居家责任)，燃气责任险-管道	3个月	BF004
			// 36㎡-80㎡(居家责任)，燃气责任险-管道	6个月	BF005
			// 36㎡-80㎡(居家责任)，燃气责任险-管道	12个月	BF006
			// 81㎡-130㎡(居家责任)	3个月	BF007
			// 81㎡-130㎡(居家责任)	6个月	BF008
			// 81㎡-130㎡(居家责任)	12个月	BF009

        }
	},
	components:{
		fwczrList,
		jjzrList,
		jtcczhList,
		InsuranceDec,
		specialAgreement,
		VDistpicker, 
	},
	created(){
				var model = {};
				model = sessionStorage.getItem('fdInsuranceData');
				model = JSON.parse(model);
				this.premiumList = model.premiumList;
				this.extendPremium = model.extendPremium;
				this.packageType = model.packageType; //套餐类型
				this.area = model.area;
				this.elevatorInsurance = model.elevatorInsurance; //电梯险
				this.hireLossInsurance = model.hireLossInsurance ; //租金损失
				this.reputationInsureance = model.reputationInsureance ;//房屋声誉损失
				this.account =	model.account ;//总金额
				this.setMeal = typeList[model.packageType];
				this.insuranceMethod = insuranceMethodList[model.currentIndex];
				this.upDataList.orderDetail.SchemeCode = setMealCode[model.packageType];//套餐code
				this.upDataList.orderDetail.ProductType = model.currentIndex;
				this.upDataList.orderDetail.PremiumCode	= areaCodeList[model.areaIndex];			
				this.cityList =[{
						values: Object.keys(city),
						className: 'column1'
						},
						{
						values: city['北京'],
						className: 'column2',
						defaultIndex: 2
						}
					]
			},
    methods :{
		 getNowFormatDate(d){
			var date;
			if(typeof(d) == "undefined"){
				date = new Date();
			}else{
				date = new Date(d); 
            	date.setFullYear(date.getFullYear()+1); 
           		date.setDate(date.getDate()-1);
			}
			var seperator1 = "-";
			var year = date.getFullYear();
			var month = date.getMonth() + 1;
			var strDate = date.getDate();
			if (month >= 1 && month <= 9) {
				month = "0" + month;
			}
			if (strDate >= 0 && strDate <= 9) {
				strDate = "0" + strDate;
			}
			var currentdate = year + seperator1 + month + seperator1 + strDate;
			return currentdate;
		},
			onApplicantConfirm(value){
				this.upData.applicant.IdentifyTypeName = value;
				this.hasApplicant = false;
				for(var i = 0 ; i < this.typeResult.length; i++){
					if(value == this.typeCode[i].name){
						this.upData.applicant.IdentifyType = this.typeCode[i].code;
					}
				}
			},
			toAddress(){
				this.hasCity = true;
			    this.mask = true;
			},
			selected(data){
				if(data.area.code){
					this.mask =false;
					this.hasCity = false;
					this.cityValue = data.province.value + ' ' + data.city.value +' ' + data.area.value;
					this.upDataList.orderDetail.AreaCode = data.area.code;
				}
			},
			closeCitySelect(){
				this.mask =false;
				this.hasCity = false;
			},
            slideSwitch(){
               if(this.sameOwner == true){
                this.sameOwner = false;
                this.upData.applicant =  { name: "", phone: "", idNo: "", idType: "", relationType: "", birthday: "", gender: "" };
               }else{
                   this.sameOwner = true;
                   this.upData.applicant = this.upData.owner
               }  
			},
			someHouse(){

				this.issomeHouse = !this.issomeHouse;
			},
			setRadioIpt(code){
				this.InsuranceCode = code; 
				this.upDataList.orderDetail.InsCorpCode = code;
			},
			onidTypeConfirm(value){
				this.upData.owner.IdentifyTypeName = value;
				this.hasidType = false;
				for(var i = 0 ; i < this.typeResult.length; i++){
					if(value == this.typeCode[i].name){
							this.upData.owner.IdentifyType = this.typeCode[i].code;
					}
				}
			},
			onCommencementDate(value){
				this.CommencementDateValue = this.Common.Public.dateString(value);
				this.hasCommencementDate = false;
			},
			
			submitInsurance(){
					var owner = {} , applicant = {} ;
				if(this.CommencementDateValue){
					this.upDataList.startDate = this.CommencementDateValue;
					this.upDataList.endDate = this.getNowFormatDate(this.upDataList.startDate);
					// console.log(this.upDataList.endDate);
				}else{
					Toast("请选择起保日期。");
					return false;
				}
				this.upDataList.amount = this.account;
				if(this.upData.owner.InsuredName){
					this.upDataList.applicantName =	this.upData.owner.InsuredName;
				}else{
					Toast("请输入投保人");
					return false;
				}
				if(this.upData.applicant.InsuredName ){
					this.upDataList.insurantName = this.upData.applicant.InsuredName;
				}else{
					Toast("请输入被投保人");
					return false ;
				}
				if(this.upData.owner.PhoneNumber){
					if(!(/^1[3456789]\d{9}$/.test(this.upData.owner.PhoneNumber))){ 
						Toast("投保人手机号码有误，请重新输入");  
						return false; 
					}
				}else{
					Toast("请输入投保人手机号");  
					return false; 
				}
				if(this.upData.applicant.PhoneNumber){
					if(!(/^1[3456789]\d{9}$/.test(this.upData.applicant.PhoneNumber))){ 
					Toast("被投保人手机号码有误，请重新输入");  
					return false; 
					}
				}else{
					Toast("请输入被投保人手机号");  
					return false; 
				}
				if(this.upData.owner.IdentifyTypeName == ""){
					Toast("请选择投保人证件类型");  
					return false; 
				}
				if(this.upData.applicant.IdentifyTypeName == ""){
					Toast("请选择投保人证件类型");  
					return false; 
				}
				
				if( this.upData.owner.IdentifyNumber){
					if(this.upData.owner.IdentifyType == "IdCard"){
						if(this.upData.owner.IdentifyNumber.length != 15 && this.upData.owner.IdentifyNumber.length != 18){
							Toast("投保人证件号错误");  
							return false; 
						}
					}
				}else{
					Toast("请输入投保人证件号");  
					return false; 
				}
				if( this.upData.applicant.IdentifyNumber){
					if(this.upData.applicant.IdentifyType == "IdCard"){
						if(this.upData.applicant.IdentifyNumber.length != 15 && this.upData.applicant.IdentifyNumber.length != 18){
							Toast("被投保人证件号错误");  
							return false; 
						}
					}
				}else{
					Toast("请输入被投保人证件号");  
					return false; 
				}
				if(this.upDataList.orderDetail.AreaCode == ""){
					Toast("请选择投保城市");  
					return false; 
				}
				if(this.upDataList.orderDetail.DetailedAddress == ""){
					Toast("请选择输入详细地址");  
					return false; 
				}

				if(this.issomeHouse && this.detailRoom != ''){
					this.upDataList.orderDetail.DetailedAddress = this.upDataList.orderDetail.DetailedAddress + this.detailRoom;
				}
				
				owner.InsuredName = this.upData.owner.InsuredName
				owner.PhoneNumber = this.upData.owner.PhoneNumber;
				owner.IdentifyType = this.upData.owner.IdentifyType;
				owner.IdentifyNumber = this.upData.owner.IdentifyNumber;
				owner.InsuredAddress = this.upData.owner.IdentifyNumber;
				owner.SerialNo = '1';

				applicant.InsuredName = this.upData.applicant.InsuredName;
				applicant.IdentifyType = this.upData.applicant.IdentifyType;
				applicant.PhoneNumber = this.upData.applicant.PhoneNumber;
				applicant.IdentifyNumber = this.upData.applicant.IdentifyNumber;
				applicant.SerialNo = '2';

				this.upDataList.orderDetail.InsCorpCode = this.InsuranceCode;

				var Extendeds = {};

				if(this.elevatorInsurance > 0){
					Extendeds.KCBZ001 = this.elevatorInsurance; 
				}
				if(this.hireLossInsurance > 0){
					Extendeds.KCBZ002 = this.hireLossInsurance;
				}
				if(this.reputationInsureance > 0){
					Extendeds.KCBZ003 = this.reputationInsureance;
				}
				this.upDataList.orderDetail.Extendeds = Extendeds;
				this.upDataList.orderDetail.RelationPeoples.push(owner);
				this.upDataList.orderDetail.RelationPeoples.push(applicant);
				var model = JSON.stringify(this.upDataList);
				sessionStorage.setItem('nonCarList',model);
				Indicator.open('加载中...');
				axiosPostAPI('/ZhongXing',this.upDataList).then(res=>{
				Indicator.close();
					if(res.status){
						this.$router.push({path:"/zxfdPage/payPage"});
					}
				})
				// {
				// 				"PhoneNumber": "string,联系电话",
				// 				"InsuredAddress": "string,详细地址",
				// 				"InsuredName": "string,名称",
				// 				"IdentifyType": "enum,证件类型",
				// 				"IdentifyNumber": "string,证件号",
				// 				"SerialNo": "string,关系人序号", //投保人=1，被保险人=2
				// 			}
				console.log(this.upDataList);	
			},
			showInsured:function(t){
				if(!this.falg){ return false;}
				this.falg = false;
				if(t == "text"){
					//console.log('123')
					this.textFalg = true;
					$(".base_msg_box").find("h3").text("保险条款");
					$(".base_msg_text").css({"border":"none"});
					$(".sbtn").hide();
				}
				var $_bg = $(".black_box");
				$_bg.show();
				var $_this = $(".base_msg_box");
				$_bg.height($(document).height());
				var animate_height = ($(window).height() - $_this.height())/2;
				$_this.animate({"margin-top":animate_height + "px","opacity":"1"},300);
				$_bg.on("touchmove",this.bodyScroll,false); //禁止页面滚动
				return;
			},
			cancelEvent:function(){
				var self = this;
				var $_this = $(".base_msg_box");
				var $_bg = $(".black_box");
				$_this.animate({"margin-top":"0px","opacity":"0"},300,
					function(){
						$_this.parent().hide();
						self.falg = true;
						$_bg.off("touchmove",self.bodyScroll,false);//恢复页面滚动
						if(self.textFalg){
							$(".base_msg_box").find("h3").text("请仔细阅读以下协议");
							$(".sbtn").show();
							$(".base_msg_text").css({"border-bottom":"1px solid #e5e5ec"});
						}
						return;
					}
				);
			},
			insuredOperate(){
				console.log(this.upDataList);	
				console.log(this.ownerModel);
				console.log(this.applicantModel);
			},
			go_back(){
				this.$router.back();
			},
		
            },	
		mounted(){
			}
        }
        </script>
		<style scoped>
		.base_msg_box>div>a.sbtn{ color:#333; background:linear-gradient(90deg,rgba(255,226,59,1) 0%,rgba(255,213,0,1) 100%);box-shadow:0px 2px 5px 0px rgba(255,192,0,0.7); border-radius: .8rem; width:80%; margin:.3rem auto;}
		.base_msg_box>h3{ padding-top:.45rem; font-size:.36rem;}
		.insuranceSetBox, .insuranceTextBottom { padding-right:.3rem;}
		.insuranceTextBottom { border:none;}
		.insuranceBttomBtnBox>.cb>div{display: flex;flex-direction: column;float: left;}
		.insuranceBttomBtnBox>.cb>div>span{ float:left; font-size:.26rem; color:#FF1212; padding-top:.1rem;}
		.insuranceBttomBtnBox>.cb>div>span>i{ font-style:normal; font-size:.4rem;}
		.headerClose{height: 30px;line-height: 45px;position: fixed;right: 10px;  width:100%;background: #fff;}
		</style>