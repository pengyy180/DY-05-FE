<template>
	<view class="page">
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
					<u-icon name="list-dot" size="50" color="#fff"></u-icon>交易记录
				 </view>
				</u-navbar>
		<view class="topher"></view>
		<!-- 订单tab -->
		<view class="order-tab" >
			<view class="tab" v-for="(item,i) in tablist" :key="i" :class="{'action':OrderType==i}" @click="onOrderTab(i)">
				<text>{{item.name}}</text>
				<text class="line"></text>
			</view>
		</view>
		<view class="recordlist">
			<view class="li" v-for="(item,index) in orderlist" @click="onOrderList(item)" :key="index">
				
				<view class="left">
					<view class="title">轮数:{{item.activityTitile}}</view>
					<view class="fj" v-if="item.roomType==1">中级房</view>
					<view class="fj" v-if="item.roomType==2">高级房</view>
					<view class="fj" v-if="item.roomType==3">VIP房</view>
				</view>
				<view class="right">
					<view class="money">
						购买{{item.orderPrice}}元
					</view>
					<view class="more"  v-if="item.orderStatus == 2 && item.zongfanhui > 0">
						返利 {{item.zongfanhui}}元
					</view>
				</view>
				
			</view>
		</view>
	</view>
</template>

<script>
	import { orderList2 } from "../../../api/gril";
	export default {
		data() {
			return {
				OrderType: 0,
				tablist: [{
				                    name: '全部',
				                }, 
								// {
				    //                 name: '昨日',
				    //             }, {
				    //                 name: '前日'
				    //             }
					],
					orderlist:[]
			}
		},
		onLoad() {
			this.checklist(this.OrderType)
		},
		methods: {
			async checklist(index){


				const  res  = await orderList2()
		
					let _this = this
					let data = res.result
					let k = []
					if(index == 0){
						_this.orderlist = data
					}else if(index == 2){
						let k = []
						data.forEach((e)=>{
							if(e.orderStatus == 1){
								k.push(e)
							}
						})
						_this.orderlist = k
					}else if(index == 1){
						let k = []
						data.forEach((e)=>{
							if(e.orderStatus == 2 && e.zongfanhui > 0){
								k.push(e)
							}
						})
						_this.orderlist = k
					}

			},
			onOrderTab(type){
				this.OrderType = type;
				this.checklist(type)
				uni.pageScrollTo({
					scrollTop: 0,
					duration: 1000
				});
				// // #ifdef H5
				// uni.redirectTo({
				// 	url: '/pages/MyOrderList/MyOrderList?type=' + type,
				// })
				// //#endif
			},
			/**
			 * 订单列表点击
			 */
			onOrderList(item){
				// uni.navigateTo({
				// 	url: '/pages/my/gameRecordIndex/gamerecorddetail?datainfo='+JSON.stringify(item) ,
				// })
			},
			goback(){
				uni.switchTab({
					url:'/pages/my/my'
				})
			},
		}
	}
</script>

<style lang="scss">
::v-deep .u-navbar__content__title{
	color:	#e6b98a;
}
.recordlist{
	width: 100%;
	margin-top: calc(130rpx + var(--status-bar-height));
	// background-color: #373536;
	font-size: 12px;
	.li{
		width: 95%;
		margin: 0 auto;
		border-bottom: 1px solid #f2f2f5;
		padding: 5% 0;
		color: #fff;
		display: flex;
		align-items: center;
		justify-content: space-between;
		.title{
			display: flex;
			justify-content: space-between;
			.name{
				font-size: 12px;
				text{
					font-size: 14px;
					margin-right: 5px;
				}
			}
			.number{
				color: #dc2037;
			}
		}
		.more{

			text-align: right;
			color: #dc2037;
		}
	}
}
/* 订单tab */
.order-tab{
	position: fixed;
	left: 0;
	top: 80rpx;
	/* #ifdef APP-PLUS */
	top: calc(44rpx + var(--status-bar-height));
	/* #endif */
	z-index: 10;
	display: flex;
	align-items: center;
	width: 100%;
	height: 100rpx;
	background: linear-gradient(90deg,#3b9dfd,#3d5179);
	.tab{
		position: relative;
		display: flex;
		align-items: center;
		justify-content: center;
		width: 25%;
		height: 100%;
		text{
			font-size: 26rpx;
			color: #fff;
		}
	}
	.action{
		text{
			// color: #e6b98a;
			font-weight: bold;
		}
		.line{
			position: absolute;
			left: 50%;
			bottom: 0;
			width: 80rpx;
			height: 6rpx;
			background: #fff;
			transform: translate(-50%,0);
		}
	}
}
.page{
  width: 100%;
  // height: 100vh;
}
.topher{
	width: 100%;
	height: 44px;
}
</style>
