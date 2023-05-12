<template>
	<view  class="page">
		<u-navbar
							leftIconSize="16px"	
							:autoBack="false"
								bgColor="#161936"
								@leftClick="goback()"
		>
					<view
		                class="u-nav-slot"
		                slot="left"
		            >
			<u-icon name="arrow-leftward" size="40" color="#fff"></u-icon>
			<u-icon name="list-dot" size="50" color="#fff"></u-icon>交易大厅
		 </view>
		</u-navbar>
		<view class="indextop"></view>
		<view class="main">
			<view class="myzj">
				<view class="left">
					<view class="t1">{{userinfo.givenAccount}}</view>
					<view class="t2">账户资金</view>
				</view>
				<view class="right">
					<!-- <view class="t1">今日盈亏</view> -->
				</view>
			</view>
			<view class="tabs">
				    <u-tabs 
					:list="list1" 
					:itemStyle="{
						width:'40%',
						height:'50px',
					}"
					:inactiveStyle="{
						color:'#fff'
					}"
					:activeStyle="{
						color:'#008eff',
						fontWeight:'bold',
						fontSize:'22px'
					}"
					lineWidth="100px"
					@click="clicktabs"></u-tabs>

			</view> 
			<view class="tablist">
				<view class="li bg3">
					<view class="left" @click="moneymin(3)">
						<view class="t1">初级房</view>
						<view class="t2">入场金额:{{config.thresholdChu}}</view>
						<view class="t3">单笔最低买入:{{config.thresholdChuMin}}</view>
					</view>

				</view>
				<view class="li bg1" @click="moneymin(0)">
					<view class="left">
						<view class="t1">中级房</view>
						<view class="t2">入场金额:{{config.thresholdInter}}</view>
						<view class="t3">单笔最低买入:{{config.thresholdInterMin}}</view>
					</view>
					<!-- <view class="right"><u-icon name="man-add-fill" color="#fff" size="28"></u-icon>交易人数:10000</view> -->
				</view>
				<view class="li bg2" @click="moneymin(1)">
					<view class="left">
						<view class="t1">高级房</view>
						<view class="t2">入场金额:{{config.thresholdAdv}}</view>
						<view class="t3">单笔最低买入:{{config.thresholdAdvMin}}</view>
					</view>
					<!-- <view class="right"><u-icon name="man-add-fill" color="#fff" size="28"></u-icon>交易人数:10000</view> -->
				</view>
			
				<view class="li bg4" @click="moneymin(2)">
					<view class="left">
						<view class="t1">VIP房</view>
						<view class="t2">入场金额:{{config.thresholdVip}}</view>
						<view class="t3">单笔最低买入:{{config.thresholdVipMin}}</view>
					</view>

				</view>
					
			</view>
		</view>
	</view>
</template>

<script>
	import { findConfig,findCustomData } from "../../api/index";
	export default {
		data() {
			return {
				list1:[{name:'USDT'}],
				config:'',
				userinfo:''
			}
		},
		onLoad() {
			this.checkfindConfig()
		},
		onShow() {
			this.checkfindConfig()
		},
		methods: {
			checkfindConfig(){
				let _this = this
				findConfig().then((res)=>{
					let data = res.result
					this.config = data
				})
				findCustomData().then(res=>{
					this.userinfo = res.result
						
							
							
				})
			},
			moneymin(item){
				let userinfo = this.userinfo
				let config = this.config

				if(item==0){
					if(userinfo.givenAccount >= config.thresholdInter){
						uni.navigateTo({
							url:'./lottery?roomType=1'
						})
					}else{
						this.showmoadl('用户余额低于'+config.thresholdInter+',无法进入')
					}
					
				}else if(item==1){
					if(userinfo.givenAccount >= config.thresholdAdv){
						uni.navigateTo({
							url:'./lottery?roomType=2'
						})
					}else{
						this.showmoadl('用户余额低于'+config.thresholdAdv+',无法进入')
					}
				}else if(item==2){
					if(userinfo.givenAccount >= config.thresholdVip){
						uni.navigateTo({
							url:'./lottery?roomType=3'
						})
					}else{
						this.showmoadl('用户余额低于'+config.thresholdVip+'，无法进入')
					}
				}else if(item==3){
					if(userinfo.givenAccount >= config.thresholdChu){
						uni.navigateTo({
							url:'./lottery?roomType=0'
						})
					}else{
						this.showmoadl('用户余额低于'+config.thresholdChu+'，无法进入')
					}
				}
			
			},
			showmoadl(msg){
				uni.showToast({
					title: msg,
					icon:'none',
					duration: 2000
				});
			},
			goback(){
				uni.navigateBack()
			}
		}
	}
</script>

<style scoped lang="scss">
	.tablist{
		width: 95%;
		margin:0 auto;
		margin-top: 10px;
		.li{
			display: flex;
			align-items: center;
			justify-content: space-between;
			color: #fff;
			font-size: 12px;
			padding: 30px 10px;
			margin-bottom: 10px;
			.t1{
				    font-size: 24px;
				    font-weight: 700;
					margin-bottom: 20px;
			}
		}
		.right{	display: flex;}
		.bg1{
			background: url('../../static/jysbg01.png') no-repeat;
			background-size:100% 100%;
		}
		.bg2{
			background: url('../../static/jysbg02.png') no-repeat;
			background-size:100% 100%;
		}
		.bg3{
			background: url('../../static/jysbg03.png') no-repeat;
			background-size:100% 100%;
		}
		.bg4{
			background: url('../../static/jysbg04.png') no-repeat;
			background-size:100% 100%;
		}
	}
	.myzj{

		height: 100px;
		background: url('../../static/pro-bg2.png') no-repeat;
		background-size:100% ;
		display: flex;
		align-items: center;
		justify-content: center;
		color: #fff;
		padding: 0 10px;
		text-align: center;
		.left{
			.t1{font-size: 33px;font-weight: 700;}
			.t2{font-size:14px;}
			}
	}
	.main{
		width: 100%;
		margin:0 auto;
		margin-top: 10px;

	}
	.indextop{
		width: 100%;
		height: 44px;
	}
.u-nav-slot{
		display: flex;
		color: #fff;
	}
</style>
