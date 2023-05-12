<template>
	<view class="page">
				<u-navbar
					leftIconSize="16px"	
					
		           title=""
				   leftText="全国"
				   leftIcon=""
		           @rightClick="rightClick"
		           :autoBack="false"
						bgColor="#161936"
						leftIconColor="#e6b98a"

		       >
			   <view
			                   class="u-nav-slot"
			                   slot="left"
			               >
			                <image class="indexlogo" src="../../static/index_logo.png" mode=""></image>
			    </view>
		       </u-navbar>
			   
			   <view class="indextop"></view>
			   <view class="main">
			   	
			  
					   <view class="banner">
							<u-swiper
										 :list="list1"
										 height="162px"
										 radius="0px"
								 ></u-swiper>
					   </view>
					  <view class="notice">
					  	<u-notice-bar 
							direction="column" 
							:text="text1"
							bgColor="transparent"
							color="#d2d3df"
							></u-notice-bar>
					  </view>
					  <view class="blist3">
							<view class="li">
								<view class="t1">BTC</view>
								<view class="t2">{{config.activityNperAdv}}期</view>
								<view class="t3">{{(Math.random()*(250000-20000)+20000).toFixed(0)}}</view>
							</view>
							<view class="li">
								<view class="t1">ETH</view>
								<view class="t2">{{config.activityNperInter}}期</view>
								<view class="t3">{{(Math.random()*(250000-20000)+20000).toFixed(0)}}</view>
							</view>
							<view class="li">
								<view class="t1">FNC</view>
								<view class="t2">{{config.activityNperVip}}期</view>
								<view class="t3">{{(Math.random()*(250000-20000)+20000).toFixed(0)}}</view>
							</view>
					  </view>
					  
					  <view class="buyc">
							<view class="left" @click="gourl(0)">
								<view class="l">
									<view class="t1">
										Trade
									</view>
									<view class="t2">交易大厅</view>
								</view>
								<view class="r">
									<image src="../../static/indexicon1.png" mode=""></image>
								</view>
							</view>
							<view class="right">
								<view class="li" @click="kefu()">
									<image src="../../static/indexicon2.png" mode=""></image>在线客服
								</view>
								<view class="li" @click="gourl(1)">
									<image src="../../static/indexicon3.png" mode=""></image>平台资讯
								</view>
							</view>
					  </view>
					  <view class="indextitle">
					  	<view class="index-scdt">市场动态</view>
					  </view>
					   <view class="describe2" >
						   <view class="titlelist">
						   	<view>交易用户</view>
						   	<view>交易品类</view>
						   	<view>成交量</view>
						   	<view>交易日期</view>
						   </view>
							<view class="content">
								
								<view class="lists">
									<view :class="{ top: animate == true }">
										<view class="list" v-for="(item,index) in datalist" :key="index">
											<view class="color2">{{item.name}}</view>
											<view>{{item.resultOdd==3?'[做多]':'[做空]'}} , {{item.resultSize==1?'[做高]':'[做低]'}}</view>
											<view class="color2">￥{{item.money}}</view>
											<view>{{systemDate}}</view>
										</view>
									</view>
								</view>
							</view>
					   </view>
					  
					 
			    </view>
			  
	</view>
</template>

<script>

	import { findMeiMeiList } from "../../api/gril";
	import { findBannerList,findConfig} from "../../api/index";
	import __config from "../../config/env.js";
	export default {
		
		data() {
			return {
				animate:false,
				timer:null,
				datalist: [],
				pplist:[],
				action: __config.basePath, // 图片显示地址
				text1:['Todays total payment: 4030658 -Todays total payment: 4030658 ','Todays total payment: 6030658 -Todays total payment: 2030658 '],
				list1:[ '../../static/banner1.jpeg',
                    '../../static/banner2.jpeg',
                    '../../static/banner3.jpeg',
					'../../static/banner4.jpeg',],
					systemDate:'',
					config:''
			}
		},
		onLoad() {
			this.timer = setInterval(this.scroll, 3000);
		},
		created() {

			this.createData()

		},
		onShow() {
			this.createData()
		},
		methods: {
			//客服
			kefu() {
				findConfig().then((res)=>{
					console.log(res.result)
					window.location.href="https://chatlink.mstatik.com/widget/standalone.html?eid="+res.result.meiqia
				})
			
			},
			gourl(item){
				if(item==0){
					uni.switchTab({
						url:'/pages/lottery/jydt'
					})
				}else if(item == 1){
					uni.navigateTo({
						url:'./indexdetails'
					})
				}
			},
			scroll() {
				this.animate = true;
				setTimeout(() => {
					this.datalist.push(this.datalist[0]);
					this.datalist.shift();
					this.pplist.push(this.pplist[0]);
					this.pplist.shift();
					this.animate = false;
				}, 1000);
			},
			createData: function(){
				
				findConfig().then((res)=>{
					let data = res.result
					this.config = data
				})
				let clist = []
				for (let i = 0; i < 30; i++) {
					let li = {}
					li.name = '***'+(Math.random()*(90000-10000)).toFixed(0)
					li.resultOdd = (Math.random()*(4-3)+3).toFixed(0)
					li.resultSize = (Math.random()*(2-1)+1).toFixed(0)
					li.money = (Math.random()*(90000-50000)).toFixed(0)
					clist.push(li)
				}
				this.datalist = clist
					
					const nowDate = new Date();
					const date = {
						year: nowDate.getFullYear(),
						month: nowDate.getMonth() + 1,
						date: nowDate.getDate(),
					}
					const newmonth = date.month>10?date.month:'0'+date.month
					const day = date.date>=10?date.date:'0'+date.date
					this.systemDate = date.year + '-' + newmonth + '-' + day
					
			},
			
			
			indextabchange(i){
				this.indextype = i;
				this.createData()
			},
			
			
		}
	}
</script>

<style lang="scss">
	.describe2{
		width: 100%;
		margin: 0 auto;
		height: 200px;
		// display: flex;
		// flex-direction: column;
		// justify-content: space-between;
		position: relative;
		overflow: hidden;
		flex-wrap: wrap;
		.titlelist{
			width: 100%;
			height: 40px;
			display: flex;
			justify-content: space-between;
			align-items: center;
			color:#cdcdd2;
			font-size: 12px;
			text-align: center;
			view{
				width: 25%;
			}
		}
		.content{
			display: flex;
			height: 100vh;
			// padding: 0 30rpx;
		}
		.lists{
			height: 400rpx;
			overflow: hidden;
			width: 100%;
			.list{
				height: 50px;
				font-size: 12px;
				color: #fff;
				display: flex;
				justify-content: space-between;
				align-items: center;
				text-align: center;
				view{
					width: 25%;
				}
				.color2{
					color:#538cbb;
				}
			}
		}
		
		.top {
		    transition: all 1s;
		    margin-top: -100rpx;
		  }
		
	}
	.indextitle{
		background: #1a243f;
		color: #f4f4f4;
		font-size: 14px;
		height: 40px;
		line-height: 40px;
		padding: 2px 10px;
		.index-scdt{
			text-indent: 10px;
			border-left: 2px solid #f4f4f4;
			line-height: 24px;
			margin-top: 8px;
		}
	}
	.buyc{
		width: 100%;
		display: flex;
		justify-content: space-between;
		margin-bottom:10px;
		.right{
			width: 43%;
			display: flex;
			flex-wrap: wrap;
			align-content: space-between;
			color: #fff;
			font-size: 14px;
			.li{
				width: 100%;
				height: 47%;
				display: flex;
				align-items: center;
				justify-content: center;
				background: #1a253f;
				border-radius: 4px;
			}
			image{
				width: 25px;
				height: 25px;
				margin-right: 10px;
			}
		}
		.left{
			width: 50%;
			background: #1a253f;
			padding: 24px 8px;
			display: flex;
			justify-content: space-between;
			border-radius: 4px;
			.t1{
				font-size: 18px;
				color: #fff;
			}
			.t2{
				color:#999;
				font-size: 13.5px;
				margin-top: 10px;
			}
			image{
				width: 41px;
				height: 41px;
			}
		}
	}
	.blist3{
		width: 100%;
		display: flex;
		margin:10px 0;
		justify-content: space-between;
		.li{
			width: 31%;
			height: 100%;
			padding: 10px 0;
			border-radius: 4px;
			background: #1a253f;
			text-align: center;
			.t1{    
				font-size: 17px;
				color: #efefef;
			}
			.t2{color:#b0b0b0;font-size: 14px;padding: 10px 0;}
			.t3{color:#4bb3e8;font-size: 12px;}
		}
	}
	::v-deep .u-icon__icon .uicon-volume{
			font-size: 16px !important;
	}
	.indextop{
		width: 100%;
		height: 10px;
	}
	.indexlogo{
		width: 110px;
		height: 30px;
	}
	//old
	.describe3{
		width: 100%;
		margin: 0 auto;
		height: 150px;
		border-radius: 0.26667rem;
		border: 0.02667rem solid #766350;
		background: linear-gradient(120deg,#212121,#313131);
		display: flex;
		padding: 10px 0px;
		flex-direction: column;
		justify-content: space-between;
		position: relative;
		overflow: hidden;
		.name{
			width: 90%;
			margin: 0 auto;
			color: #e6b98a;
		}
		.content{
			width: 90%;
			margin: 0 auto;
			font-size: 12px;
			color: #fff;
		}
	}
	
	.indexlist{
		width: 100%;
		height: 100%;
		.li{
			    width: 100%;
			    height:150px;
			    border-radius: 10px;
			    border:1px solid #766350;
			    margin-bottom: 10px;
			    background: linear-gradient(120deg,#212121,#313131);
			    display: flex;
			    justify-content: space-between;
				align-items: center;
				position: relative;
				overflow: hidden;
				font-size: 12px;
				.left{
					margin-left: 10px;
					.title{
						background: linear-gradient(90deg,#efcba7,#dcab76);
						color: #8d5825;
						height: 20px;
						line-height: 20px;
						padding: 0 10px;
						border-radius: 20px;
						text-align: center;
						margin-bottom: 10px;
					}
					.name{
						color: #929292;
					}
					.icon{
						color: #929292;
						display: flex;
						margin-top: 10px;
						.iconmagrin{
							margin: 0 5px;
						}
					}
				}
			    .right{
					image{
						width: 100px;
						height: 100px;
						border-radius: 50%;
						margin-right: 10px;
					}
				}
				.sign-i{
						position: absolute;
					    right: -30px;
					    top: 5px;
					    background: #ff6056;
					    font-size: 12px;
					    padding: 5px 30px;
					    font-style: normal;
					    transform: rotate(45deg);
					    color: #fff;
				}
		}
	}
	::v-deep .u-navbar__content__title{
		color:	#e6b98a;
	}
	.indextab{
		width: 100%;
		height: 40px;
		display: flex;
		font-size: 12px;
		margin: 10px 0;
		.li{
			padding: 0 5px;
			margin: 0 5px;
			height: 20px;
			line-height: 20px;
			border-radius:10px;
			background: linear-gradient(90deg,#5b606c,#2b2b2b);
		}
		.active{
			background: linear-gradient(#fde3ca,#e7b486);
			color: #8a5623;
		}
	}
	.page{
		width: 100%;
		// height: 100vh;
		background-color: #050925;
	}
	.main{
		width: 96%;
		margin:0 auto;
		margin-top: 10px;
	}
	.banner{
		width: 100%;
		margin: 0 auto;
		margin-top: 44px;
		margin-bottom: 10px;
	}
	.describe{
		width: 100%;
		margin: 0 auto;
		height: 134px;
		border-radius: 0.26667rem;
		border: 0.02667rem solid #766350;
		background: linear-gradient(120deg,#212121,#313131);
		display: flex;
		padding: 10px 0px;
		flex-direction: column;
		justify-content: space-between;
		position: relative;
		overflow: hidden;
		.ul{
			width: 95%;
			height: 83px;
			margin: 0 auto;
			display: flex;
			color: #f3c7a2;
			.li:nth-child(1){
				width: 35%;
				border-right: 3px solid #e6b98a;
				text-align: center;
				font-size: 0.8rem;
				.title{
					font-size: 12px;
					font-weight: 700;
				}
			}
			.li:nth-child(2){
				width: 65%;
				color: #c8c9cc;
				font-size: 12px;
				padding-left: 5px;
				line-height: 1.5rem;
				text{
					font-size: 16px;
					    color: #f8c693;
				}
			}
		}
		.bottom{
			width: 95%;
			margin: 0 auto;
			color: #f3c7a2;
			text-align: center;
			font-size: 0.8rem;
			line-height: 1.5rem;
		}
	}
	.describe:before , .describe:after{
		content: "";
		position: absolute;
		width: 20px;
		height: 300px;
		background: linear-gradient(90deg,rgba(74,70,70,.29),transparent);
		transform: rotate(50deg);
	}
	.describe:after{
		left: 50%;
	}
</style>
