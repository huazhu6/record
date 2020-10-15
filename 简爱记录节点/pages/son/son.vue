<template>
	<view class="son">
		<!-- <view>
			<view>子级节点</view>
			<view>{{goods.data.childLabel[252].product.product_name}}</view>
			<view>{{goods.data.childLabel[252].product.product_description}}</view>

			<view v-for="(item,index) in goods.data.childLabel[252].product.specification" :key="index">
				<text>{{item.key}}:</text>
				<text>{{item.value}}</text>
			</view>

			<view class="page-section-spacing">
				<swiper class="swiper" indicator-dots="true" autoplay="true" interval="5000" duration="1500">
					<swiper-item v-for="i in goods.data.childLabel[252].product.imgs" :key="i">
						<image :src="i" mode="aspectFill"></image>
					</swiper-item>
				</swiper>
			</view>
		</view> -->
		
		<view v-for="i in goods.data.childLabel" :key='i' class="list">
			<view>子级节点数量{{i.labels.length}}</view>
			<view>{{i.product.product_name}}</view>
			<view>{{i.product.product_description}}</view>
			
			<view v-for="(item,index) in i.product.specification" :key="index">
				<text>{{item.key}}:</text>
				<text>{{item.value}}</text>
			</view>
			
			<view class="page-section-spacing">
				<swiper class="swiper" indicator-dots="true" autoplay="true" interval="5000" duration="1500">
					<swiper-item v-for="i in i.product.imgs" :key="i">
						<image :src="i" mode="aspectFill"></image>
					</swiper-item>
				</swiper>
			</view>
		</view>
		
		<!-- 按钮 -->
		<view class="but">
			<button type="default" @click="out()">扫码删除</button>
			<button type="default" @click="dell()">删除清空</button>
		</view>


	</view>
</template>

<script>
	export default {
		data() {
			return {
				token:'',
				qr:[],   //存放要删除的子节点
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
							"252": {
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
					this_.empty()  //遍历所有数组
				}
			});

			uni.getStorage({
				key: 'token',
				success(e) {
					console.log(e)
					this_.token = e.data
				}
			})
		},

		methods: {
			// 获取网址参数
			getQueryString: function(url, name) {
			
				var reg = new RegExp('(^|&|%|/?)' + name + '=([^&|%|/?]*)(&|%|/?|$)', 'i')
			
				var r = url.substr(1).match(reg)
			
				if (r != null) {
			
					// console.log("r = " + r)
			
					// console.log("r[2] = " + r[2])
			
					return r[2]
			
				}
			
				return null;
			
			},
			// 遍历所有节点删除
			empty(){
				
				for(var i in this.goods.data.childLabel){
					this.goods.data.childLabel[i].labels.forEach(item=>{
						this.qr.push(item)
					})
				}
				
			},
			// 扫码删除单个节点
			out(){
				var this_=this
				wx.scanCode({
					success(res){
						console.log(res)
						// 判断是否为我们的普通小程序码
						console.log(res.result.substr(0, 23))
						if (res.result.substr(0, 23) == 'https://www.gotrack.app'||res.result.substr(0, 23) == 'https://www.gotrack.com') {
							res.result = this_.getQueryString(res.result, 'id');
						};
						this_.qr=[]   //清空数组
						this_.qr.push(res.result)
						this_.dell()
					}
				})
			},
			// 删除节点
			dell(){
				var this_ = this
				var token_ = 'Token ' + this_.token
				uni.request({
					url: "https://test.gotrack.com.cn/api/updateLabelRel/",
					data: {
						"childLabels": this_.qr, //子节点
					},
					header: {
						'Authorization': token_
					},
					method: 'POST',
					success: res => {
						console.log(res)
						if (res.data.status == true) {
							uni.showToast({
								title: '删除成功'
							})
							// 返回菜单页 延迟2秒执行
							setTimeout(function() {
								uni.navigateBack({
									delta: 2
								})
							}, 1500)
				
						} else {
							uni.showToast({
								title: '删除失败',
								icon: 'none'
							})
						}
					}
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
	
	.son {
		width: 100%;
		height: 100%;
		background: #a2e7ff;
		text-align: center;
		position: relative;
	}
	.son>.list{
		background: #FFFFFF;
		margin: 20px;
		display: inline-block;
		border-radius: 10px;
		box-shadow: 0 0 5px #808080;
	}
	.son>.but{
		position: absolute;
		bottom: 30px;
		text-align: center;
		z-index: 8;
		height: 50px;
		left: 0;
		width: 100%;
	}
	.but>button{
		width: 40%;
		margin: 0 10px;
		line-height: 50px;
		background: #FFFFFF;
		display: inline-block;
	}
</style>
