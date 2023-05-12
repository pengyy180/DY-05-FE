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
				<u-icon name="list-dot" size="50" color="#fff"></u-icon>平台资讯
			 </view>
			</u-navbar>
			
			<u-collapse
			    @change="change"
			    @close="close"
			    @open="open"
			  >
			  <view class="indextop"></view>
			    <u-collapse-item
			      title="一.BitAsset简介"
			      name="Docs guide"
			    >
			      <text class="u-collapse-content">享负盛名的BitAsset为全球华人和企业提供具备多样性、专业性、前瞻性的金融产品与服务，业务范围包括国际结算与支付、企业外汇管理、证券投资、财富管理、及金融 IT 解决方案等。经过快速与稳健的发展，集团现已在澳大利亚悉尼和墨尔本、新西兰奥克兰、加拿大多伦多、中国香港、台北等多个国际城市设有全资公司，并实现全球全天无间断 24 小时电子交易。</text>
			    </u-collapse-item>
			    <u-collapse-item
			      title="二.短期定投简介"
			      name="Variety components"
			    >
			      <text class="u-collapse-content">为满足新手小白对数字货币的理解与认识基于平台的运行模式，BitAsset策划团队利用ETH网络的可去中心化、可编程性、稀缺性、不可控制性等。技术团队通过模型搭建对ETH每3分钟的趋势变化推测出下一个三分钟的涨跌幅度，区块链各项数据的不间断记录、分析等尖端技术方法得出矿力云计算数据的三个数字，以便可以直观的操作ETH在大盘的涨跌预测，从而进行操作ETH的买进和卖空的平台运行模式下的短期投资的理财产</text>
			    </u-collapse-item>
			    <u-collapse-item
			      title="三.中长期定投简介"
			      name="Numerous tools"
			    >
			      <text class="u-collapse-content">借鉴Alipay中余额宝的无门槛加入、实时返利等，综合 网络金融稳定性较好的操作理念制定出BitAsset的理财方向。（注：所有取出到账时间均为T＋1）</text>
			    </u-collapse-item>
				<u-collapse-item
				  title="四.平台资讯"
				  name="Numerous tools"
				>
				  <text class="u-collapse-content">本平台使用usdt为唯一计量单位</text>
				</u-collapse-item>

			  </u-collapse>
		
	</view>
</template>

<script>
	import { findMeiMei } from "../../api/gril";
	import __config from "../../config/env.js";
	export default {
		data() {
			return {
				 mmdetails:'',
				action: __config.basePath, // 图片显示地址
				imgdata:[]
			}
		},
		onLoad(option) {

		},
		methods: {
			open(e) {
			        // console.log('open', e)
			      },
			      close(e) {
			        // console.log('close', e)
			      },
			      change(e) {
			        // console.log('change', e)
			      },
			goback(){
				uni.switchTab({
					url:'/pages/index/index'
				})
			}
		}
	}
</script>

<style lang="scss">
	::v-deep .u-collapse-item__content__text[data-v-2675fb21]{
		font-size: 16px;
		color: #cbcbd6;
		background-color: #151936;
		padding-bottom: 20px;
	}
	::v-deep .u-cell__body{
		background-color: #31395e;
	}
	::v-deep .u-cell__title-text{
		color:#fff;
	}
	.indextop{
		width: 100%;
		height: 50px;
	}
	
.page{
  width: 100%;
  // height: 100vh;

}
// .topher{
// 	width: 100%;
// 	height: 44px;
// }
</style>
