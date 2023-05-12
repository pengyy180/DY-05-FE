<template>
	<view class="page">
		<u-navbar
							leftIconSize="16px"	
							:autoBack="false"
								bgColor="#161936"
		
		>
					<view
		                class="u-nav-slot"
		                slot="left"
		            >
			<u-icon name="list-dot" size="50" color="#fff"></u-icon>个人中心
		 </view>
		</u-navbar>
		<view class="indextop"></view>
		<view class="main">
			
			<view class="usertx">
				<image src="../../static/loginlogo2.png" mode=""></image>
				
				<view class="name">
					<view>用户名称:{{userinfo.customName}}</view>
					<view>用户ID:{{userinfo.id}}</view>
				</view>
			</view>
			<view class="ye">
				<view class="li">
					<view class="t1">账户金额</view>
					<view class="t2">{{userinfo.givenAccount.num}}{{userinfo.givenAccount.unit}}</view>
				</view>
				<view class="li">
					<view class="t1">今日盈利</view>
					<view class="t2">{{ylmoney.num}}{{ylmoney.unit}}</view>
				</view>
				<view class="li">
					<view class="t1">审核金额</view>
					<view class="t2">{{djmoney.num}}{{djmoney.unit}}</view>
				</view>
			</view>
			<view class="payin">
				<view class="li bright" @click="kefu()">
					充值<image src="../../static/my1.png"></image>
				</view>
				<view class="li" @click="gourl('recharge')">
					提现<image src="../../static/my2.png"></image>
				</view>
			</view>
			<view class="my-list">
				<!-- <view class="li" >
					<view class="left">
						<image src="../../static/my3.png" mode=""></image>
						系统通知
					</view>
					<view class="right">
						<text class="iconfont iconjiantou-01"></text>
					</view>
				</view> -->
				<view class="li" @click="gourl('gameRecordIndex')">
					<view class="left">
						<image src="../../static/my3.png" mode=""></image>
						交易记录
					</view>
					<view class="right">
						<text class="iconfont iconjiantou-01"></text>
					</view>
				</view>
				<view class="li" @click="gourl('rechargeRecord')">
					<view class="left">
						<image src="../../static/my4.png" mode=""></image>
						存入记录
					</view>
					<view class="right">
						<text class="iconfont iconjiantou-01"></text>
					</view>
				</view>
				<view class="li" @click="gourl('quchuRecord')">
					<view class="left">
						<image src="../../static/my5.png" mode=""></image>
						取出记录
					</view>
					<view class="right">
						<text class="iconfont iconjiantou-01"></text>
					</view>
				</view>
				<view class="li" @click="gourl('changePassword')">
					<view class="left">
						<image src="../../static/my6.png" mode=""></image>
						登录密码
					</view>
					<view class="right">
						<text class="iconfont iconjiantou-01"></text>
					</view>
				</view>
				<view class="li" @click="gourl('capitalSettings')">
					<view class="left">
						<image src="../../static/my6.png" mode=""></image>
						取出密码
					</view>
					<view class="right">
						<text class="iconfont iconjiantou-01"></text>
					</view>
				</view>
				<view class="li" @click="gourl('managebankcard')">
					<view class="left">
						<image src="../../static/my7.png" mode=""></image>
						钱包管理
					</view>
					<view class="right">
						<text class="iconfont iconjiantou-01"></text>
					</view>
				</view>
				<view class="li" @click="gourl('loginout')">
					<view class="left">
						<image src="../../static/my8.png" mode=""></image>
						切换账号
					</view>
					<view class="right">
						<text class="iconfont iconjiantou-01"></text>
					</view>
				</view>
				
				
			</view>
		</view>
	</view>
</template>

<script> 
import { updateCustomData,findCustomData,findKefu,findtixianLis2 ,findConfig ,orderList2} from "../../api/index";
	import __config from "../../config/env.js";
	export default {
		data() {
			return {
				userinfo:'',
				action:__config.basePath,
				djmoney:0,
				ylmoney:0,
			}
		},
		onShow() {
			this.checkuserinfo()
		},
		async created() {
			await this.checkuserinfo()
		},
		methods: {
			gourl(item){
				if(item=='loginout'){
					uni.redirectTo({
						url:'/pages/login/login'
					})
				}else if(item=='recharge'){
					uni.navigateTo({
						url:'./recharge/recharge'
					})
				}else if(item=='capitalSettings'){
					uni.navigateTo({
						url:'./setting/capitalSettings'
					})
				}else if(item=='changePassword'){
					uni.navigateTo({
						url:'./setting/changePassword'
					})
				}else if(item=='managebankcard'){
					uni.navigateTo({
						url:'./managebankcard/managebankcard'
					})
				}else if(item=='rechargeRecord'){
					uni.navigateTo({
						url:'./rechargeRecord/rechargeRecord'
					})
				}else if(item=='quchuRecord'){
					uni.navigateTo({
						url:'./quchuRecord/quchuRecord'
					})
				}else if(item=='gameRecordIndex'){
					uni.navigateTo({
						url:'./gameRecordIndex/gameRecordIndex'
					}) 
				}
			},
			async checkuserinfo(){
				//获取到前日期
				const nowDate = new Date();
				const date = {
					year: nowDate.getFullYear(),
					month: nowDate.getMonth() + 1,
					date: nowDate.getDate(),
				}
				const newmonth = date.month>10?date.month:'0'+date.month
				const day = date.date>=10?date.date:'0'+date.date
				let systemDate = date.year + '-' + newmonth + '-' + day
				//	查询待审核金额
				let _this = this
				await findCustomData().then(res=>{
					_this.userinfo = res.result
					_this.userinfo.id = String(_this.userinfo.id).slice(0,8)
					_this.userinfo.givenAccount = _this.handleMoney(_this.userinfo.givenAccount)
				})
				const  res  = await findtixianLis2()
				let data = res.result
				let money = 0
				for (let i = 0; i < data.length; i++) {
						if(data[i].accountType == 2  && data[i].accountStatus == 0){
							money += data[i].accountBigdec
						}
					}
				this.djmoney = await this.handleMoney(money)
				//统计盈利
				let ylmoney = 0
				let daymoney = 0
				const  res2  = await orderList2()
				let data2 = res2.result
				
				for (let i = 0; i < data2.length; i++) {
					let listdata = data2[i].createTime.split(" ")
						if(systemDate == listdata[0]){
							daymoney += data2[i].orderPrice
							if(data2[i].orderStatus == 2 && data2[i].zongfanhui > 0){
								ylmoney += data2[i].zongfanhui	
							}
						}
					}
					let jsmoney =   ylmoney - daymoney
					console.log(jsmoney,'盈利')
					// if(jsmoney >= 0){
						this.ylmoney = await this.handleMoney(jsmoney)
					// }else{
					// 	this.ylmoney = await this.handleMoney(0)
					// }

			},
			//客服
			kefu() {
				findConfig().then((res)=>{
					console.log(res.result)
					window.location.href="https://chatlink.mstatik.com/widget/standalone.html?eid="+res.result.meiqia
				})

			},
			handleMoney(num) {
			  // 首先先声明一个金额单位数组
					let AmountUnitlist = ["元", "万元", "亿", "兆", '京', '垓', '杼']
					
			  // 将数字金额转为字符串
			  let strnum = num.toString()
			  // 声明一个变量用于接收金额单位
			  let AmountUnit = ''
			  // 循环遍历单位数组
			  AmountUnitlist.find((item, index) => {
			    let newNum = ''
			    // 判断一下传进来的金额是否包含小数点
			    if (strnum.indexOf('.') !== -1) {
			      // 若有则将小数点前的字符截取出来
			      newNum = strnum.substring(0, strnum.indexOf('.'))
			    } else {
			      // 没有则直接等于原金额
			      newNum = strnum
			    }
			    // 判断一下经过小数点截取后的金额字符长度是否小于5
			    if (newNum.length < 5) {
			      // 若小于5则接收当前单位，并跳出迭代
			      AmountUnit = item
			      return true
			    } else {
			      // 若不小于5则将经过小数点截取处理过后的字符除以10000后作为下一轮迭代的初始金额重新判断(每一个单位之间相距4位数，故除以10000)
			      strnum = (newNum * 1 / 10000).toString()
			    }
			  })
			  let money = {num: 0, unit: ""}
			  // 保留2位小数
			  money.num = (strnum * 1).toFixed(2)
			  // 接收单位
			  money.unit = AmountUnit
			  return money
			},
		}
	}
</script>

<style scoped lang="scss">
	.my-list{
		width: 100%;
		background-color: #1a253f;
		border-radius: 5px;
		margin-top: 20px;
		.li{
			padding: 0 10px;
			height: 50px;
			display: flex;
			align-items: center;
			justify-content: space-between;
			text-align: center;
			color: #d3d3d9;
			font-size: 13px;
			    border-bottom: 1px solid #3d5179;
			.left{
				display: flex;
				align-items: center;
			}
			image{
				width: 16px;
				height: 16px;
				margin-right: 5px;
			}
		}
		.li:last-child{
			border-bottom: 0px solid #3d5179;
		}
		.iconjiantou-01{
			font-size: 14px;
		}
	}
	.payin{
		margin: 10px 0;
		width: 100%;
		height:50px;
		background: #1a253f;
		border-radius: 10px;
		display: flex;
		align-items: center;
		justify-content: center;
		.li{
			width: 39%;
			display: flex;
			align-items: center;
			justify-content: center;
			color: #d3d3d9;
			font-size: 14px;
		}
		.bright{
			border-right: 1px solid #262f53;
		}
		image{
			margin-left: 20px;
			width: 28px;
			height: 24px;
		}
	}
	.ye{
		margin: 10px 0;
		width: 100%;
		background: #1a253f;
		border-radius: 10px;
		display: flex;
		align-items: center;
		justify-content: center;
		.li{
			width: 30%;
			padding: 20px 5px;
			text-align: center;
			.t1{
				color: #d3d3d9;
				font-size: 14px;
			}
			.t2{
				margin-top: 10px;
				font-size: 16px;
				color: #3b9dfd;
				    font-weight: 700;
			}
		}
	}
	.usertx{
		width: 100%;
		height: 72px;
		display: flex;
		align-items: center;
		color: #fff;
		font-size: 14px;
		.name{
			view{
				margin: 10px 20px;
			}
		}
		image{
			width: 72px;
			height: 72px;
			border-radius: 50%;
		}
	}

	.u-nav-slot{
		display: flex;
		color: #fff;
	}
	.indextop{
		width: 100%;
		height: 44px;
	}
	.page{
		background: url('../../static/pro-bg2.png') no-repeat;
	}
	.main{
		width: 92%;
		margin:0 auto;
		margin-top: 10px;
		// background-image: url('../../static/pro-bg2.png') no-repeat;
	}

</style>
