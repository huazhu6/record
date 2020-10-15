<template>
	<view class="menu">
		
		<view @click="jump()">批次号记录</view>
		
		<view @click="jump_()">标签绑定</view>
		
		<view @click="scan()">查询产品信息</view>
		
		<view @click="jump__()">订单号记录</view>
		
	</view>
</template>

<script>
	export default{
		methods:{
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
			
			// 跳转批次号
			jump(){
				uni.navigateTo({
					url: '/pages/record/record'
				})
			},
			// 跳转标次号
			jump_(){
				uni.navigateTo({
					url: '/pages/record_/record_'
				})
			},
			// 扫描二维码查询产品信息
			scan(){
				var this_=this
				wx.scanCode({
					success(res){
						console.log(res)
						// 判断是否为我们的普通小程序码
						console.log(res.result.substr(0, 23))
						if (res.result.substr(0, 23) == 'https://www.gotrack.app'||res.result.substr(0, 23) == 'https://www.gotrack.com') {
							res.result = this_.getQueryString(res.result, 'id');
						};

						uni.request({
							url: "https://test.gotrack.com.cn/api/queryLabelRel/",
							header: {
								// 'Authorization': token_
								'content-type': 'application/json'
							},
							data:{
								'label':res.result
							},
							method: 'GET',
							success: res => {
								console.log(res)
								if (res.data.status==true) {
									uni.showToast({
										title: '查询成功'
									})
									// 保存商品信息
									uni.setStorage({
										key:'goods',
										data:res,
									}),
									// 跳到子节点界面 延迟2秒执行
									setTimeout(function() {
										uni.navigateTo({
											url:'/pages/son_/son_'
										})
									}, 1500)
						
								} else {
									uni.showToast({
										title: '查询失败',
										icon: 'none'
									})
								}
							}
						})
						
						
						
						
					}
				})
			},
			
			// 跳转订单号记录
			jump__(){
				uni.navigateTo({
					url:'/pages/record__/record__'
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
	
	.menu {
		width: 100%;
		height: 100%;
		background: #a2e7ff;
		text-align: center;
	}
	.menu view {
		display: inline-block;
		width: 80%;
		line-height: 50px;
		border-radius: 30px;
		/* border: 1px solid #333333; */
		background: #FFFFFF;
		margin: 10px 0;
	}
	.menu>view:nth-child(1){
		margin-top: 60%;
	}
</style>
