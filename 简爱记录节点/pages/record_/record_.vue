<template>
	<view class="QR">
		<!-- 浮窗 -->
		<view>
			<image src="../../static/QR.png" mode=""></image>
			<view>扫描父节点，进行查询</view>
			<button @click="scan()">扫描 <text>( {{num}} )</text></button>
			<view>扫描子节点，进行查询</view>
			<button @click="scan_()">扫描 <text>( {{num_}} )</text></button>
			<button class="node" v-if='num_&&num!==0' @click="node()">记录</button>
		</view>
	</view>

</template>

<script>
	export default {
		data() {
			return {
				token: '', //token
				place: '', //地点
				latitude: '', //坐标
				longitude: '',
				father: '', //保存父级扫描数据
				qr: [], //保存子级扫描数据
				num: 0, //父级扫描次数
				num_: 0 //子级已经扫码次数
			}
		},
		onShow() {

			// 获取保存的token 坐标地点等
			var this_ = this

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

			//父级扫描
			scan() {
				var this_ = this
				wx.scanCode({
					success(res) {
						// 判断是否为我们的普通小程序码
						console.log(res.result.substr(0, 23))
						if (res.result.substr(0, 23) == 'https://www.gotrack.app'||res.result.substr(0, 23) == 'https://www.gotrack.com') {
							res.result = this_.getQueryString(res.result, 'id');
						};
						console.log(res)
						this_.father = res.result
						console.log(this_.father)
						if (this_.num == 0) {
							this_.num++
						} else {
							uni.showToast({
								title: '父级仅能扫描一次，将覆盖之前扫描结果',
								icon: 'none'
							});
						}

					}
				})
			},
			// 子级扫描
			scan_() {
				var this_ = this
				wx.scanCode({
					success(res) {
						// 判断是否为我们的普通小程序码
						console.log(res.result.substr(0, 23))
						if (res.result.substr(0, 23) == 'https://www.gotrack.app'||res.result.substr(0, 23) == 'https://www.gotrack.com') {
							res.result = this_.getQueryString(res.result, 'id');
						};
						var i = this_.num_
						// 首次扫描直接添加
						this_.qr.push(res.result)
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




						console.log(res)
						console.log(this_.qr)
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
					url: "https://test.gotrack.com.cn/api/updateLabelRel/",
					data: {
						"childLabels": this_.qr, //子节点
						"parentLabel": this_.father, //父节点
					},
					header: {
						'Authorization': token_
					},
					method: 'POST',
					success: res => {
						console.log(res.data.status)
						if (res.data.status == true) {
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
		/* padding-bottom: 50px; */
		color: #a7a7a7;
		/* border-bottom: 1px solid #a7a7a7; */
	}

	.QR>view>button {
		width: 180px;
		line-height: 50px;
		border-radius: 20px;
		margin-top: 20px;
		margin-bottom: 20px;
		background: #a2e7ff;
	}

	.QR>view>button:nth-of-type(3) {
		margin-bottom: 20px;
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
