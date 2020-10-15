<template>
	<view class="father">
		<view>
			<!-- 父节点标题 -->
			<view>父级节点</view>
			<view>{{goods.data.parentProduct.product_name}}</view>
			<view>{{goods.data.parentProduct.product_description}}</view>
			
			<view v-for="(item,index) in goods.data.parentProduct.specification" :key="index">
				<text>{{item.key}}:</text>
				<text>{{item.value}}</text>
			</view>
			
			<view class="page-section-spacing">
				<swiper class="swiper" indicator-dots="true" autoplay="true" interval="5000" duration="1500">
					<swiper-item v-for="i in goods.data.parentProduct.imgs" :key="i">
						<image :src="i" mode="aspectFill"></image>
					</swiper-item>
				</swiper>
			</view>
			
		</view>
		
		<button @click="jump()">查看子节点</button>

	</view>
</template>

<script>
	export default {
		data() {
			return {
				goods: {
					"data": {
						"parentProduct": {
							"product_number": "SC2040R",
							"product_name": "Swell四维卫浴SC2040R-P挂式直冲横排水座便器",
							"product_description": "韩国原装进口主板\r\n韩国原装进口主板,采用国际先进的SMT贴片工艺,自动化流水线无尘生产,保证产品性能可靠稳定。\r\n进口EXL进水阀\r\n采用原装进口世界顶级EXL进水阀,其膜片质量优异,可在-40℃-200℃下正常使用,可耐10万次高压水冲击,有效防止智能马桶喷头滴水、漏水现象。\r\n进口TOK阻尼器\r\n采用原装进口世界第一TOK阻尼器,做工精细,性能稳定,寿命长,原装进口的阳尼硅油克服了一般硅油遇高温膨胀,遇低温凝结的缺点.",
							"specification": [{
									"key": "安装方式",
									"value": "挂墙式"
								},
								{
									"key": "安装方式",
									"value": "挂墙式"
								},
								{
									"key": "排污方式",
									"value": "墙排"
								},
								{
									"key": "盖板类型",
									"value": "快拆缓降盖板"
								},
								{
									"key": "釉面",
									"value": "特白釉+自洁釉"
								},
								{
									"key": "主要材质",
									"value": "陶瓷底座+工程塑料（PP)缓降盖板"
								}
							],
							"imgs": [
								"https://gotracktests3.s3.cn-northwest-1.amazonaws.com.cn/%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_20200728194140_cynXUP7.png",
								"https://gotracktests3.s3.cn-northwest-1.amazonaws.com.cn/%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_20200728201257.png",
								"https://gotracktests3.s3.cn-northwest-1.amazonaws.com.cn/%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_20200728201308.png"
							]
						},
						"childLabel": {
							"251": {
								"labels": [
									"25115982518695345934pAn"
								],
								"product": {
									"product_number": "SC2040R",
									"product_name": "Swell四维卫浴SC2040R-P挂式直冲横排水座便器",
									"product_description": "韩国原装进口主板\r\n韩国原装进口主板,采用国际先进的SMT贴片工艺,自动化流水线无尘生产,保证产品性能可靠稳定。\r\n进口EXL进水阀\r\n采用原装进口世界顶级EXL进水阀,其膜片质量优异,可在-40℃-200℃下正常使用,可耐10万次高压水冲击,有效防止智能马桶喷头滴水、漏水现象。\r\n进口TOK阻尼器\r\n采用原装进口世界第一TOK阻尼器,做工精细,性能稳定,寿命长,原装进口的阳尼硅油克服了一般硅油遇高温膨胀,遇低温凝结的缺点.",
									"specification": [{
											"key": "安装方式",
											"value": "挂墙式"
										},
										{
											"key": "安装方式",
											"value": "挂墙式"
										},
										{
											"key": "排污方式",
											"value": "墙排"
										},
										{
											"key": "盖板类型",
											"value": "快拆缓降盖板"
										},
										{
											"key": "釉面",
											"value": "特白釉+自洁釉"
										},
										{
											"key": "主要材质",
											"value": "陶瓷底座+工程塑料（PP)缓降盖板"
										}
									],
									"imgs": [
										"https://gotracktests3.s3.cn-northwest-1.amazonaws.com.cn/%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_20200728194140_cynXUP7.png",
										"https://gotracktests3.s3.cn-northwest-1.amazonaws.com.cn/%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_20200728201257.png",
										"https://gotracktests3.s3.cn-northwest-1.amazonaws.com.cn/%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_20200728201308.png"
									]
								}
							}
						},
						"label": "9ee063-1598251869-534593",
						"msg": "successfully query",
						"status": true
					},
					"header": {
						"Date": "Thu, 17 Sep 2020 07:08:58 GMT",
						"Server": "WSGIServer/0.2 CPython/3.8.2",
						"Content-Type": "application/json",
						"Vary": "Accept, Accept-Language, Cookie",
						"Allow": "GET, HEAD, OPTIONS",
						"Content-Language": "en",
						"X-Frame-Options": "SAMEORIGIN",
						"Content-Length": "2899"
					},
					"statusCode": 200,
					"cookies": [],
					"errMsg": "request:ok"
				},

			}
		},
		
		onShow() {
			var this_=this
			uni.getStorage({
				key: 'goods',
				success(e) {
					console.log(e);
					this_.goods=e.data
				}
			});
		},
		methods:{
			// 跳转子节点页面
			jump(){
				uni.navigateTo({
					url:'/pages/son/son'
				})
			}
			
		}

	}
</script>

<style>
	html,
	body,
	#app {
		height: 100%;
	}

	.father {
		width: 100%;
		height: 100%;
		background: #a2e7ff;
		text-align: center;
		position: relative;
	}
	.father>view{
		background: #FFFFFF;
		margin: 20px;
		display: inline-block;
		border-radius: 10px;
		box-shadow: 0 0 5px #808080;
	}
	.father>button{
		position: fixed;
		bottom: 40px;
		left: 5%;
		width: 90%;
		height: 50px;
	}
</style>
