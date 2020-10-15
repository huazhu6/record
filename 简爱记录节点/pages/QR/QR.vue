<template>
	<view class="QR">
		<!-- 浮窗 -->
		<view>
			<image src="../../static/QR.png" mode=""></image>
			<view>扫描二维码，进行查询</view>
			<button @click="scan()">扫描 <text>( {{num_}} )</text></button>
			<button class="node" v-if='num_!==0' @click="node()">记录</button>
		</view>
	</view>

</template>

<script>
	export default {
		data() {
			return {
				num: '', //批次号
				token: '', //token
				place: '', //地点
				latitude: '', //坐标
				longitude: '',
				qr: [], //保存扫描数据
				num_: 0 //已经扫码次数
			}
		},
		onShow() {

			// 获取保存的token 坐标地点 批次号等
			var this_ = this
			uni.getStorage({
				key: 'num',
				success(e) {
					console.log(e)
					this_.num = e.data
				}
			});
			uni.getStorage({
				key: 'token',
				success(e) {
					console.log(e)
					// this_.token = 'token '+e.data
					this_.token = e.data
				}
			});
			uni.getStorage({
				key: 'place',
				success(e) {
					console.log(e)
					this_.place = e.data
				}
			});
			uni.getStorage({
				key: 'latitude',
				success(e) {
					console.log(e)
					this_.latitude = e.data
				}
			});
			uni.getStorage({
				key: 'longitude',
				success(e) {
					console.log(e)
					this_.longitude = e.data
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
			// 扫描
			scan() {
				var this_ = this
				wx.scanCode({
					success(res) {
						// 判断是否为我们的普通小程序码
						console.log(res.result.substr(0, 23))
						if (res.result.substr(0, 23) == 'https://www.gotrack.app'||res.result.substr(0, 23) == 'https://www.gotrack.com') {
							res.result = this_.getQueryString(res.result, 'id');
						}
						console.log(res.result)
						var i = this_.num_
						// 首次扫描直接添加
						this_.qr.push(res.result)
						console.log(this_.qr)
						// 遍历去重
						this_.qr = [...new Set(this_.qr)]
						this_.num_ = this_.qr.length
						// 扫描了同一二维码提示用户
						if (i == this_.num_) {
							uni.showToast({
								title: '请勿重复扫描同一二维码',
								icon: 'none'
							})
						
						}
						// 保存扫描数据
						// uni.setStorage({
						// 	key: 'QR',
						// 	data: res.result,
						// })

					}
				})
			},

			// 记录节点
			node() {
				uni.showToast({
					icon: 'loading'
				})
				var this_ = this
				var token_ = 'Token ' + this_.token
				// console.log('token是'+this_.token)
				uni.request({
					url: "https://test.gotrack.com.cn/api/provenance-node/",
					data: {
						'label': this.qr, //扫描标签
						'location': this_.place,
						'latitude': this_.latitude,
						'longitude': this_.longitude,
						'label_type': 0,
						'extra_info': {
							'batchNumber': 'FHG-1515:15DJIL'
						}
					},
					header: {
						'Authorization': token_
					},
					method: 'POST',
					success: res => {
						console.log(res)
						if (res.data.msg == 'successful.') {
							uni.showToast({
								title: '记录成功'
							})
							// 返回上一页 延迟2秒执行
							setTimeout(function() {
								uni.navigateBack({
									delta: 1
								})
							}, 1500)

						} else {
							uni.showToast({
								title: '记录失败',
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
	.QR {
		text-align: center;
	}

	.QR>view {
		display: inline-block;
		width: 240px;
		text-align: center;
		box-shadow: 0 0 20px #d1d1d1;
		border-radius: 20px;
		margin-top: 20%;
		padding: 40px;
	}

	.QR>view>image {
		width: 150px;
		height: 150px;
		margin: 20px 0;
	}

	.QR>view>view {
		padding-bottom: 50px;
		color: #a7a7a7;
		border-bottom: 1px solid #a7a7a7;
	}

	.QR>view>button {
		width: 180px;
		line-height: 50px;
		border-radius: 20px;
		margin-top: 40px;
		background: #a2e7ff;
	}

	.QR>view>button>text {
		display: inline-block;
		font-size: 16px;
		margin-left: 8px;
	}

	.QR .node {
		margin-top: 10px;
	}
</style>
