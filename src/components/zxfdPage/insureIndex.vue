<template>
	<div style="background:#F5F5F5;">
        <div class="nweInsuranceBg"></div>
        <div class="nweInsuranceColumn">
        	<div class="nweInsuranceTagBox cb">
            	<!--current-->
				<a href="javascript:void(0)" :class="{current: index == currentIndex}" v-for=" (item , index) in newInsuranceItem" :key="index"  @click="nweInsuranceTag(item,index)">
					{{item.name}}
				</a>
            	<!-- <a class="current" href="javascript:void(0)" @click="nweInsuranceTag($event)">按栋投保</a>
                <a class="" href="javascript:void(0)" @click="nweInsuranceTag($event)">按户投保</a> -->
            </div>
            <div style="padding-left:.3rem">
                <h4 class="nweInsuranceTitle">
                	<span>投保面积</span>
                </h4>
                <div class="insuranceSetBox newWidth" id="setArea_1">
                	 <a @click="setArea(item,index)" :data-area="item.value" :class="{current: index == areaIndex}" v-for="(item,index) in areaList" :key="index">{{item.name}}</a>
                </div>
                <div class="insuranceSetBox newWidth" id="setArea_2" style="display:none;">
					 <a @click="setWenxArea(item,index)" :data-area="item.value" :class="{current: index == areaWenxIndex}" v-for="(item,index) in areaWenxList" :key="index">{{item.name}}</a>
                </div>
				<p style="color:#999999;padding-right:4%;">必须按照对应的面积区间进行投保，否则将视为不足额投保，对于所有保障内容将按照投保方案上限面积数/实际面积的比例进行赔付。</p>
				 <h4 class="nweInsuranceTitle" style="margin-top:24px;">
                	<span>选择套餐</span>
                </h4>
                <div class="insuranceSetBox" id="setPackage_1">
                	 <a @click="setTancan(_item,_index)" :class="{current: _index == tancanIndex}" v-for="(_item,_index) in TancanList" :key="_index">{{_item.name}}</a>
                </div>
                <div class="insuranceSetBox" id="setPackage_2" style="display:none;">
                    <a  class="current"><input name="package" type="radio" value="3" id="aRadio_4"><label for="aRadio_4">温馨版</label></a>
                </div>
				 <div style="padding-right:.3rem;">
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
                </table>
                </div>
                <h4 class="nweInsuranceTitle">
                	<span>扩展保障</span>
                </h4>
                <ul class="newInsuranceTextList">
                	<li class="cb pr">
                    	<span>电梯责任保障</span> <a class="InTextMsgBtn" href="javascript:void(0)" data-text='赔偿受保楼栋内电梯或升降机造成的第三者人身伤害损失。'></a>
                        <div class="setQuantityBox">
                        	<a class="iNreduceBtn" :class="{opacityBtn : elevatorInsurance == 0 }" href="javascript:void(0)" @click="elevatorSub"></a>
                            <span>{{elevatorInsurance}}份</span><input name="lift" type="hidden" value="0">
                            <a class="iNaddToBtn" :class="{opacityBtn : elevatorInsurance == 5 }" href="javascript:void(0)" @click="elevatorAdd"></a>
                        </div>
                    </li>
                    <li class="cb">
                    	<span>租金损失保障</span> <a class="InTextMsgBtn" href="javascript:void(0)" data-text='因发生主险内保险事故导致该房屋无法继续出租的，赔偿至可正常出租前的实际租金损失。'></a>
                        <div class="setQuantityBox">
                        	<a class="iNreduceBtn" :class="{opacityBtn : hireLossInsurance == 0 }" href="javascript:void(0)" @click="hireLossSub"></a>
                            <span>{{hireLossInsurance}}份</span><input name="rent" type="hidden" value="0">
                            <a class="iNaddToBtn" :class="{opacityBtn : hireLossInsurance == 5 }" href="javascript:void(0)" @click="hireLossAdd"></a>
                        </div>
                    </li>
                    <li class="cb">
                    	<span>租房声誉损失津贴保障</span> <a class="InTextMsgBtn" href="javascript:void(0)" data-text='因自杀、犯罪行为发生人身死亡事件，赔偿租房声誉损失。'></a>
                        <div class="setQuantityBox">
                        	<a class="iNreduceBtn" :class="{opacityBtn : reputationInsureance == 0 }" href="javascript:void(0)" @click="reputationSub"></a>
                            <span>{{reputationInsureance}}份</span><input name="reputation" type="hidden" value="0">
                            <a class="iNaddToBtn" :class="{opacityBtn : reputationInsureance == 5 }" href="javascript:void(0)" @click="reputationAdd"></a>
                        </div>
                    </li>
                </ul>
                <h4 class="nweInsuranceTitle">
                	<span>保障内容</span>
                </h4>
                <div style="padding-right:.3rem;">
                	<table class="nweInsuranceTab">
					<tr>
						<th style="border: 0;">可扩展保障内容</th>
						<th>保险责任</th>
						<th>限额(元)</th>
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
                
                <div class="insuranceTextBottom cb">
                    <span><i class="totalAmount">{{account}}</i>元/年</span>
                </div>
            </div>
        </div>
        <div class="insuranceColumn" style="padding-bottom:1.36rem;">
		<van-sticky>
        	<div class="insuranceNavBox" style="height:1.02rem;">
            	<div class="insuranceNav" id="insuranceNav">
					<a href="javascript:void(0)" @click="cutoverTag(item,index)" :class="{current : index == tagIndex}"  v-for="(item,index) in cutoverTagList " :key="index">{{item}}</a>
			    </div>	
            </div>
			</van-sticky>
            <div class="insuranceTagBox">
            	<div class="iTag_1" v-show="isTage_1" style="">
                	<div class="iImgs_1 iImgs"></div>
                    <div class="iImgs_2 iImgs">
                    	<div style="width:6.3rem; height:2.58rem; margin:0 auto;">
                        	<div class="slide cb" id="slideBanner">
								<van-swipe :autoplay="3000" style="height: 2.58rem;" show-indicators indicator-color="#CCCCCC" >
									<van-swipe-item><img style="width:100%" src="../../assets/images/insurance/example_1.jpg"></van-swipe-item>
									<van-swipe-item><img style="width:100%" src="../../assets/images/insurance/example_2.jpg"></van-swipe-item>
								</van-swipe>
                                <div class="dot"></div>
                            </div>
                        </div>
                    </div>
                    <div class="iImgs_3 iImgs"></div>
                    <div class="iImgs_4 iImgs"></div>
                </div>
                <div class="iTag_2" v-show="isTage_2" >
                	<h4 class="insuranceTitle" style="border-bottom:1px solid #eee; padding-left:.3rem;">
                        <span>特别约定</span>
                    </h4>
                    <div class="insuranceTextBox">
                        <p>1.投保人/被保险人：房屋出租人（房东、二手房东）</p>
                        <p>2.房屋类型：城中村（按栋/户）、农民房（按栋/户）、商品房（按户）</p>
                        <p>3.保险期限：一年</p>
                        <p><b>约定内容</b></p>
                        <p>1.出租人已与各承租人签订房屋租赁合同的，保险人认可该出租人作为本保险单的被保险人；出租人已与转租人签订房屋承包租赁合同且转租人已与各承租人签订房屋租赁合同的，保险人认可该出租人及转租人作为本保险单的共同被保险人。</p>
                        <p>2.第三者财产损失及出租人家庭财产损失每次事故财产损失免赔300元或和核定损失金额的5%，两者以高者为准；第三者人身伤亡每次事故医疗费用免赔100元或核定损失金额的5%，两者以高者为准。</p>
                        <p>3.本保险单载明地址的房屋在保险期限内发生高空坠物、水管爆裂意外事故，造成第三者人身伤亡或财产损失，依法应由被保险人承担的损害赔偿责任，保险人在本保险单载明限额内承担赔偿责任。</p>
                        <p>4.本保险单扩展承保在本保险单载明地址的房屋内因斗殴、吸毒、自杀、疾病或治安刑事案件等原因导致第三者人身伤亡，依法应由被保险人应支付的医疗费用、丧葬费用以及其他费用，每套房屋累计责任限额5万元。</p>
                        <p>5.兹经双方同意，本保险扩展承保本保险单明细表中列明的地点范围内的电梯、升降机在正常使用过程中发生意外事故造成第三者人身伤亡或财产损失时依法应由被保险人承担的经济赔偿责任。被保险人应持有有关政府部门颁发的电梯、升降机合格证书,并保证对电梯、升降机有合格的技术人员定期进行检查和维修。</p>
                        <p>但以下情况本保单不负责赔偿：</p>
                        <p>（一） 由于自然灾害和意外事故等原因造成电梯本身的损坏；</p>
                        <p>（二） 劳动管理部门对影响安全的有关问题提出限期整改后仍未改正的；</p>
                        <p>（三） 保险责任事故发生后引起的各种间接损失；</p>
                        <p>（四） 因电梯超载引起的损失和费用。</p>
                        <p>本项扩展责任累计赔偿责任限额50万元/份。</p>
                        <p>6. 在保险期间内，保险单载明地址的出租房屋因发生主险列明的保险事故导致该房屋无法继续出租的，保险人对被保险人所遭受的在受损房屋合理恢复至可居住状态（不好于受损前的实际状态）前的实际租金损失负责赔偿。</p>
                        <p>保险人按出险前 30 日保险房屋的有效租约约定的租金计算每日租金，并以此作为日租金赔偿金额。</p>
                        <p>赔偿天数为保险事故发生后受损房屋的实际修复天数扣除本保险单列明的每次事故绝 对免赔天数。</p>
                        <p>租金损失赔偿金额 ＝日租金赔偿金额 X 赔偿天数，但最高不超过设置的累计赔偿限额。</p>
                        <p>日租金赔偿金额最高赔偿标准：500元/天</p>
                        <p>最高赔偿期限：100天</p>
                        <p>相对免赔天数5天，即无法继续出租天数少于5天，本保单不予以赔偿，超过5天，按照实际无法继续出租天数赔偿，但最长不超过最高赔偿期限。</p>
                        <p>租金损失赔偿金额累计赔偿限额5万元/份。</p>
                        <p>7. 保险期限内，本保险单载明的的出租住宅房屋内因自杀、犯罪行为发生人身死亡事件，保险人按照保险单载明的金额向被保险人承担赔偿责任。索赔时需提供公安机关提供的案件证明。租房声誉损失津贴保障金额5万元份。</p>
                        <p>8.本保单仅承保住宅房屋，经营性质场所不在保障范围内。</p>
                        <p>9.必须按照对应的面积区间进行投保，否则将视为不足额投保，对于所有保障内容将按照投保方案上限面积数/实际面积的比例进行赔付。</p>
                        <p>10.如投保人投保多个地址，须单独进行投保，不可合计面积进行选择，否则本保单不承担保险责任。</p>
                        <p>11.发生保险事故，如被保险人无法提供房屋所有权证或不动产权证书，街道办等政府部门能够出具房屋或土地权属证明的，保险人按照保单约定承担赔偿责任。</p>
                    </div>
                </div>
                <div class="iTag_3" v-show="isTage_3">
                	<h4 class="insuranceTitle" style="border-bottom:1px solid #eee; padding-left:.3rem;">
                        <span>理赔流程</span>
                    </h4>
                    <ul class="claimProcessList">
                    	<li class="cb">
                        	<span style="background:#FFCC00;">1</span>
                        	<div>
                            	<i class="claimIcon_1"></i> 拨打报案电话 <p style="font-size:.3rem; line-height:140%; position: absolute; top:.13rem; right:.4rem; text-align:right;">95518(中国人保)<br>95500(中国太保)</p>
                            </div>
                        </li>
                        <li class="cb">
                        	<span style="background:#00CEFF;">2</span>
                        	<div>
                            	<i class="claimIcon_2"></i> 事故勘察和损失确认
                            </div>
                        </li>
                        <li class="cb">
                        	<span style="background:#25D200;">3</span>
                        	<div>
                            	<i class="claimIcon_3"></i> 提交索赔材料
                            </div>
                        </li>
                        <li class="cb">
                        	<span style="background:#CBCF03;">4</span>
                        	<div>
                            	<i class="claimIcon_4"></i> 赔款计算和审核
                            </div>
                        </li>
                        <li class="cb">
                        	<span style="background:#00D095;">5</span>
                        	<div>
                            	<i class="claimIcon_5"></i> 领取赔款
                            </div>
                        </li>
                    </ul>
                    <div style=" border-top:.17rem solid #F5F5F5">
                    	<h4 class="insuranceTitle" style="border-bottom:1px solid #eee; padding-left:.3rem;">
                            <span>了解详情</span>
                        </h4>
                        <div class="insuranceTextBox">
                        	<p class="t52">事故发生后，首先请保护现场并采取必要的紧急施救措施，然后请按以下步骤进行理赔：</p>
                            <p class="t52">1、请立即拨打保险公司报案电话95518(中国人保)/95500(中国太平洋保险)或有条件的情况下通过传真等方式向保险公司报案，保险公司理赔服务人员将向您询问出险情况，协助安排救助，告知后续理赔处理流程并指导您拨打报警电话，紧急情况下请您先拨打报警电话。</p>
                            <p class="t52">2、事故勘察和损失确认在您的协助下，保险公司理赔人员或委托的公估机构、技术鉴定机构、海外代理人到事故现场勘察事故经过，了解涉及的损失情况，查阅和初步收集与事故性质、原因和损失情况等有关的证据和资料，确认事故是否属于保险责任，必要时委托专门的技术鉴定部门或科研机构提供专业技术支持。保险公司将指导您填写出险通知书（索赔申请书），向您出具索赔须知。与您共同对保险财产的损失范围、损失数量、损失程度、损失金额等损失内容、涉及的人身伤亡损害赔偿内容、施救和其他相关费用进行确认，确定受损财产的修复方式和费用，必要时委托具备资质的第三方损失鉴定评估机构提供专业技术支持。</p>
                            <p class="t52">3、提交索赔材料请根据保险公司书面告知您的索赔须知内容提交索赔所需的全部材料，保险公司及时对您提交的索赔材料的真实性和完备性进行审核确认，索赔材料不完整的情况下保险公司将及时通知您补充提供有关材料，对索赔材料真实性存在疑问的情况下保险公司将及时进行调查核实。</p>
                            <p class="t52">4、赔款计算和审核在您提交的索赔材料真实齐全的情况下，保险公司根据保险合同的约定和相关的法律法规进行保险赔款的准确计算和赔案的内部审核工作，并与您达成最终的赔偿协议。</p>
                            <p class="t52">5、领取赔款：保险公司根据与您商定的赔款支付方式和保险合同的约定向您支付赔款。 补充说明：因第三者对保险标的的损害而造成保险事故的，在保险公司根据保险合同的约定和相关的法律法规向您支付赔款后请您签署权益转让书并协助保险公司向第三方进行追偿工作。</p>
                        </div>
                    </div>
               	</div>
            </div>
        </div>
        <div class="insuranceBttomBtnBox">
        	<div class="cb">
            	<span><i class="totalAmount">{{account}}</i>元/{{currentIndex == 0 ? "栋" : "户"}}/年</span>
				<!-- <a href="javascript:void(0)" @click="showInsured('text')">《保险条款》</a> -->
                <a class="insuranceBttomBtn" href="javascript:void(0)" @click="showInsured()">立即投保</a>
            </div>
        </div>
        
        <!--同意协议并投保-->
        <div class="black_box" style="display:none;">
            <div class="base_msg_box">
            	<a class="closedBtn" @click="cancelEvent()"><span></span></a>
            	<h3>请仔细阅读以下协议</h3>
            	<div class="base_msg_text">
                	<a @click="showFwczrList = true" style="font-weight:500;">《房屋出租人责任险条款》</a>
                    <a @click="showDszssList = true" style="font-weight:500;">《第三者损失保险条款》</a>
                    <a @click="showJtcczhList = true" style="font-weight:500;">《家庭财产综合保险条款》</a>
                    <a @click="showJjzrList = true" style="font-weight:500;">《居家责任保险条款》</a>
                </div>
            	<div class="cb"><a class="sbtn" href="javascript:void(0)" @click="insuredOperate">同意协议并投保</a></div>
            </div>
        </div>

		<van-popup v-model="showFwczrList" round style="height:80%" position="bottom">
			<a class="closedPopupBtn" @click="closeFwczrList"><span></span></a>
			<fwczrList></fwczrList> 
		</van-popup>
		
		<van-popup v-model="showDszssList"  round style="height:80%" position="bottom">
			<a class="closedPopupBtn" @click="showDszssList = false"><span></span></a>
			<sanzexianList></sanzexianList> 
		</van-popup>

		<van-popup v-model="showJtcczhList"  round style="height:80%" position="bottom">
			<a class="closedPopupBtn" @click="showJtcczhList = false"><span></span></a>
			<jtcczhList></jtcczhList> 
		</van-popup>

		<van-popup v-model="showJjzrList"  round style="height:80%" position="bottom">
			<div class="closedPopupBtn" @click="showJjzrList = false"><span></span></div>
			<jjzrList></jjzrList> 
		</van-popup>
        </div>
        <!--同意协议并投保 end-->
</template>
  <script >
   import "./../../assets/css/zxfd/base.scss";
   import "./../../assets/js/rem.js";
   import fwczrList from "./fwczrList.vue";
   import jjzrList from "./jjzrList.vue";
   import jtcczhList from "./jtcczhList.vue";
   import sanzexianList from "./sanzexianList.vue";
		var moneyData = [
			[2000,3000,4000,5500], //标准版
			[3000,4000,5000,6500], //豪华版
			[5000,6000,7000,8500], //尊享无忧版
			[298,698,998,1198]	 //温馨版
		]
		
		var iAmountData = [
			[300,300,150,20,5,50,10,10,10],
			[600,600,200,25,5,100,20,20,20],
			[1000,1000,200,30,5,150,30,30,30],
			[300,300,150,20,5,50,10,10,10]
		]
		//console.log(moneyData[0][3])
		//带出去数据
		var boothArea = ""; //投保总面积
		var securityLimit = ""; //保障额度
		var iPackage = ""; //套餐
		var insuranceFee = 0;
		
		var insurance_type; //投保方式：1-按栋，2-按户
		var insurance_set; //套餐类型：1-标准，2-豪华，3-尊享，4-温馨版
		var booth_area; //面积代码
		var lift_liability; //电梯责任保障
		var rent_money; //租金损失保障
		var reputational_risk; //声誉损失保障
        //选择扩展保份数
 export default {
    data(){
        return{
				account:'2000',
				newInsuranceItem:[{name:'按栋投保',value:'allhouse'},{name:'按户投保',value:'house'}],
				currentIndex:0,
				TancanList:[{name:'标准版',value:'0'},{name:'豪华版',value:'1'},{name:'尊享无忧版',value:'2'}],
				tancanIndex:0,		
				tancanValue:'',
				areaList:[{name:'500~1000㎡',value:'500-1000'},{name:'1001~2000㎡',value:'1001-2000'},{name:'2001~3000㎡',value:'2001-3000'},{name:'3001~5500㎡',value:'3001-5500'}],
				areaIndex:0,
				areaValue:'500~1000㎡',
				moneyData : [
					[2000,3000,4000,5500], //标准版
					[3000,4000,5000,6500], //豪华版
					[5000,6000,7000,8500], //尊享无忧版
					[298,698,998,1198]	 //温馨版
				],
				areaWenxList:[{name:'1~50㎡',value:'1~50'},{name:'51~90㎡',value:'51~90'},{name:'91~130㎡',value:'91~130'},{name:'131~200㎡',value:'131~200'}],
				areaWenxIndex:0,
				areaWenxValue:'1~50㎡',
				elevatorInsurance:0, //电梯责任保障
				hireLossInsurance:0, //租金损失保障
				reputationInsureance:0, //租房声誉损失津贴保障
				cutoverTagList:['特别约定','产品特色','理赔服务'],
				tagIndex:1,
				isTage_1:true,
				isTage_2:false,
				isTage_3:false,
				falg:true,
				textFalg:false,
				premiumList:[300,300,150,20,5,50,10,10,10],
				extendPremium:[0,0,0],
				showFwczrList:false,
				showDszssList:false,
				showJtcczhList:false,
				showJjzrList:false,
        }
	},
	components:{
		fwczrList,
		jjzrList,
		jtcczhList,
		sanzexianList,
	},
    methods :{
		closeFwczrList(){
			this.showFwczrList = false;
		},
		tofwczList(){
			this.showFwczrList = true;
		},
		//选择按栋按户
		 nweInsuranceTag(item,index){
			 var $_setPackage_1 = $("#setPackage_1");
			var $_setPackage_2 = $("#setPackage_2");
			var $_setArea_1 = $("#setArea_1");
			var $_setArea_2 = $("#setArea_2");
			if(index == this.currentIndex){
				return false;					
			}
			else{
				this.currentIndex = index;
			}
			if(index == 0){		
				this.account = this.moneyData[this.tancanIndex][this.areaIndex]	+ this.elevatorInsurance *350 + this.hireLossInsurance * 100 + this.reputationInsureance * 100 ;	
				this.premiumList = iAmountData[this.tancanIndex];
				$_setPackage_1.show();
				$_setPackage_2.hide();
				$_setArea_1.show();
				$_setArea_2.hide();
			}else{
				this.account = this.moneyData[3][this.areaWenxIndex] + this.elevatorInsurance *350 + this.hireLossInsurance * 100 + this.reputationInsureance * 100 ;
				this.premiumList = iAmountData[3];		
				$_setPackage_1.hide();
				$_setPackage_2.show();
				$_setArea_1.hide();
				$_setArea_2.show();
			}
		},

		setTancan(item,index){ //按动套餐选择
			if(index == this.tancanIndex){
				return false;					
			}
			else{
				this.tancanIndex = index;
				this.account = this.moneyData[index][this.areaIndex] + this.elevatorInsurance *350 + this.hireLossInsurance * 100 + this.reputationInsureance * 100 ;
				this.premiumList = iAmountData[index];
			}
			this.tancanValue = item.value;
		},
		setArea(item,index){ //按动面积选择
			if(index == this.areaIndex){
				return false;					
			}
			else{
				this.areaIndex = index;
				this.account = this.moneyData[this.tancanIndex][index] + this.elevatorInsurance *350 + this.hireLossInsurance * 100 + this.reputationInsureance * 100 ;
			}
			this.areaValue = item.name;
		},
		setWenxArea(item,index){ //温馨版面积选择
			if(index == this.areaWenxIndex){
				return false;					
			}
			else{
				this.areaWenxIndex = index;
				this.account = this.moneyData[3][index] + this.elevatorInsurance *350 + this.hireLossInsurance * 100 + this.reputationInsureance * 100 ;
			}
			this.areaWenxValue = item.name;
		},
		elevatorSub(){ //电梯责任险减
			if(this.elevatorInsurance > 0){
				this.elevatorInsurance = this.elevatorInsurance - 1;
				this.extendPremium[0] =  50 * this.elevatorInsurance;
				this.account = this.account * 1 - 350;
			}else{
				return
			}
		},
		elevatorAdd(){ //电梯责任加
			if(this.elevatorInsurance < 5){
				this.elevatorInsurance = this.elevatorInsurance + 1;
				this.extendPremium[0] =  50 * this.elevatorInsurance;
				this.account = this.account * 1 + 350;
			}else{
				return
			}
		},
		hireLossSub(){ //租金损失保障减
			if(this.hireLossInsurance > 0){
				this.hireLossInsurance = this.hireLossInsurance - 1;
				this.extendPremium[1] =  5 * this.hireLossInsurance;
				this.account = this.account * 1 - 100;
			}else{
				return
			}
		},
		hireLossAdd(){ //租金损失保障加
			if(this.hireLossInsurance < 5){
				this.hireLossInsurance = this.hireLossInsurance + 1;
				this.extendPremium[1] = 5 * this.hireLossInsurance;
				this.account = this.account * 1 + 100;
			}else{
				return
			}
		},
		reputationSub(){ //租金损失保障减
			if(this.reputationInsureance > 0){
				this.reputationInsureance = this.reputationInsureance - 1;
				this.extendPremium[2] =  5 * this.reputationInsureance;
				this.account = this.account * 1 - 100;
			}else{
				return
			}
		},
		reputationAdd(){ //租金损失保障加
			if(this.reputationInsureance < 5){
				this.reputationInsureance = this.reputationInsureance + 1;
				this.extendPremium[2] = 5 * this.reputationInsureance;
				this.account = this.account * 1 + 100;
			}else{
				return
			}
		},
		 cutoverTag(item,index){ //切换tag
			if(index == this.tagIndex){return};
			this.tagIndex = index;
			if(index == 0){
				this.isTage_1 = false;
				this.isTage_2 = true;
				this.isTage_3 = false;
			}else if(index == 1){
				this.isTage_1 = true;
				this.isTage_2 = false;
				this.isTage_3 = false;
			}else{
				this.isTage_1 = false;
				this.isTage_2 = false;
				this.isTage_3 = true
			}	
		},
		calculate(){
				var lift = $("input[name = lift]").val();
				var rent = $("input[name = rent]").val();
				var reputation = $("input[name = reputation]").val();
				console.log(lift*350 + rent*100 + reputation*100 + insuranceFee)
				$(".totalAmount").text(lift*350 + rent*100 + reputation*100 + insuranceFee)
			},
			 //同意协议并投保
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
			if(this.currentIndex == 0){ //按栋投保
				var model = {};
				model.packageType = this.tancanIndex //套餐类型
				model.currentIndex = this.currentIndex;
				// this.areaIndex //投保面积
				model.area = this.areaValue ;
				model.areaIndex = this.areaIndex ;
				model.elevatorInsurance = this.elevatorInsurance; //电梯险
				model.hireLossInsurance = this.hireLossInsurance; //租金损失
				model.reputationInsureance = this.reputationInsureance ;//房屋声誉损失
				model.account = this.account ;//总金额
				model.premiumList = this.premiumList; // 保障金额
				model.extendPremium =this.extendPremium ;//电梯险等附加险金额
			}else if(this.currentIndex == 1){
				var model = {};
				model.packageType = 3 ;//套餐类型
				model.area = this.areaWenxValue; 
				model.areaIndex = this.areaWenxIndex;
				model.elevatorInsurance = this.elevatorInsurance; //电梯险
				model.hireLossInsurance = this.hireLossInsurance ;//租金损失
				model.reputationInsureance = this.reputationInsureance ;//房屋声誉损失
				model.account = this.account //总金额
				model.premiumList = this.premiumList // 保障金额
				model.extendPremium =this.extendPremium //电梯险等附加险金额
				model.currentIndex = this.currentIndex;
			}
				sessionStorage.setItem('fdInsuranceData',JSON.stringify(model));
				this.$router.push({path:'/zxfdPage/orderInformation'});
				this.cancelEvent();
		},
		bodyScroll:function(event){
				event.preventDefault();
			},
            },
            created(){
			// var appId = "cx7190761141531rzi";
			// localStorage.setItem("appId",appId);
			
			},	
			mounted(){
				$(document.body).on("click", "#inpoTextMsgBox", function(e){
						$("#inpoTextMsgBox").remove();
						$(".inpoTextMsgBox").remove();
					});		
					//展示黄色图标内容
				$(".InTextMsgBtn").click(function(){
					$(document.body).append('<div id="inpoTextMsgBox"></div><div class="inpoTextMsgBox"><span class="s1"></span><span class="s2"></span><p></p></div>');
					var $_mgsBox = $(".inpoTextMsgBox");
					var $_mgsp = $(".inpoTextMsgBox>p");
					var $_idBox =$("#inpoTextMsgBox")
					var $_this = $(this);
					$_mgsp.html($_this.data("text"));
					var lt_1=$_mgsBox.width();
					var lt_2=$_this.offset().left;
					var tops=$_this.offset().top + $_this.height()*1.5;
					if(lt_1>=lt_2){
						$_mgsBox.css({"top":+tops+"px","left":+lt_2+"px"});
						$(".inpoTextMsgBox>span").css({"left":"3px","right":""});
						//$_mgsBox.width(lt_2);
					}else{
						var tt= $(window).width()-lt_2-$(this).width();
						$_mgsBox.css({"top":+tops+"px","right":+tt+"px","left":""});
						$(".inpoTextMsgBox>span").css({"left":"","right":"3px"});
					}
				});

			}
        }
        </script>
		<style scoped>
		.base_msg_box>div>a.sbtn{ color:#333; background:linear-gradient(90deg,rgba(255,226,59,1) 0%,rgba(255,213,0,1) 100%);box-shadow:0px 2px 5px 0px rgba(255,192,0,0.7); border-radius: .8rem; width:80%; margin:.3rem auto;padding: .2rem;}
		.base_msg_box>h3{ padding-top:.45rem; font-size:.36rem;}
		.insuranceSetBox, .insuranceTextBottom { padding-right:.3rem;}
		.insuranceTextBottom { border:none;}
		</style>