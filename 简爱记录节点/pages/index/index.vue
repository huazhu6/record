<template>
	<view class="content">

		<image src="../../static/top.png" mode=""></image>

		<input type="text" placeholder="请输入账号" v-model="name" />
		<input password="true" placeholder="请输入密码" v-model="pwd" />

		<button type="default" @click="login()">登录</button>


	</view>
</template>

<script>
	// 引入高德
	import amap from '../../common/amap-wx.js';

	export default {
		data() {
			return {
				name: '', //存账号
				pwd: '', //存密码

				amapPlugin: null,
				key: '480a478b9f3e324a46985532d6173499',
				addressName: '',
				weather: {
					hasData: false,
					data: []
				}
			}
		},


		// 用户点击右上角分享
		onShareAppMessage: function() {},


		onShow() {
			// 获取地点
			this.isGetLocation();

			// 高德
			// this.amapPlugin = new amap.AMapWX({
			// 	key: this.key
			// });
			// this.getRegeo();

			// 获取用户名与密码
			var this_ = this
			uni.getStorage({
				key: 'name',
				success(e) {
					// console.log(e)
					this_.name = e.data
				}
			});
			uni.getStorage({
				key: 'pwd',
				success(e) {
					this_.pwd = e.data
				}
			})

		},
		methods: {

			isGetLocation(a = "scope.userLocation") { // 3. 检查当前是否已经授权访问scope属性
				var _this = this;
				uni.getSetting({
					success(res) {
						if (!res.authSetting[a]) { //3.1 每次进入程序判断当前是否获得授权，如果没有就去获得授权，如果获得授权，就直接获取当前地理位置
							_this.getAuthorizeInfo()
						} else {
							_this.getLocationInfo()
						}
					}
				});
			},


			// 位置授权
			getAuthorizeInfo() {
				// uni.showToast({
				// 	title: '获取地理位置信息',
				// 	// icon: 'none'
				// 	icon: 'loading'
				// });

				const that = this;
				uni.authorize({
					scope: 'scope.userLocation',
					success() { // 允许授权
						that.getLocationInfo();
					},
					fail() { // 拒绝授权
						console.log("你拒绝了授权，无法获得周边信息")
					}
				})
			},
			// 获取地理位置
			getLocationInfo() {
				var this_ = this
				uni.getLocation({
					type: 'wgs84',
					success(res) {
						console.log(res);
						// 保存坐标点
						uni.setStorage({
								key: 'latitude',
								data: res.latitude,
							}),
							uni.setStorage({
								key: 'longitude',
								data: res.longitude,
							})
						// 获取中文地理位置信息
						let latitude, longitude;
						latitude = res.latitude.toString();
						longitude = res.longitude.toString();

						uni.request({
							header: {
								"Content-Type": "application/text"
							},
							url: 'https://apis.map.qq.com/ws/geocoder/v1/?location=' + latitude + ',' + longitude +
								'&key=FWBBZ-4GJWU-3VZVO-2NGEV-7RDTS-JDF66',
							success(re) {
								console.log("中文位置")
								var china = re.data.result.address_component

								this_.addressName = china.nation + '.' + china.province + '.' + china.city
								console.log(this_.addressName)
								// 记录中文地点
								uni.setStorage({
									key: 'place',
									data: this_.addressName,
								})
								// 显示地理位置信息
								// uni.showToast({
								// 	title: this_.addressName,
								// 	icon: 'none'
								// 	// icon: 'loading'
								// });

							}
						});



					}
				});
			},
			// 发送登录请求
			login() {
				// 显示加载中
				uni.showToast({
					title: '加载中',
					// icon: 'none'
					icon: 'loading'
				});

				var this_ = this
				uni.request({
					url: "https://test.gotrack.com.cn/api/login/",
					data: {
						'password': this_.pwd,
						'email_or_number': this_.name
					},
					header: {
						'ACCEPT-LANGUAGE': 'zh-hans'
					},
					method: 'POST',
					success: res => {
						// console.log(res)
						if (res.data.msg == '登录成功') {
							// 保存账号和密码
							uni.setStorage({
									key: 'pwd',
									data: this_.pwd,
								}),
								uni.setStorage({
									key: 'name',
									data: this_.name,
								})
							// 保存token
							uni.setStorage({
									key: 'token',
									data: res.data.data.token,
								}),
								// 跳转记录节点
								uni.navigateTo({
									url: '/pages/menu/menu'
								})

						} else {
							// 清空账号和密码
							uni.setStorage({
									key: 'pwd',
									data: '',
								}),
								uni.setStorage({
									key: 'name',
									data: '',
								}),
								uni.showToast({
									title: '账号与密码不匹配',
									icon: 'none'
								});

						}
					}
				})

			},

			// 高德获取地理位置信息
			getRegeo() {
				// uni.showLoading({
				// 	title: '获取地理位置中'
				// });
				this.amapPlugin.getRegeo({
					success: (data) => {
						console.log(data)
						this.addressName = data[0].regeocodeData.addressComponent.country + '.' + data[0].regeocodeData.addressComponent
							.province + '.' +
							data[0].regeocodeData.addressComponent.district;
						console.log(this.addressName)
						// 记录中文地点
						uni.setStorage({
								key: 'place',
								data: this.addressName,
							}),
							// 显示地理位置信息
							uni.showToast({
								title: this.addressName,
								icon: 'none'
								// icon: 'loading'
							});
						uni.hideLoading();
					}
				});
			}


		}
	}
</script>

<style>
	.content {
		text-align: center;
	}

	.content>image {
		margin-top: 10%;
		width: 300px;
		height: 300px;
	}

	.content>input {
		width: 80%;
		height: 40px;
		border-radius: 20px;
		border: 1px solid #555555;
		margin: 15px 10%;
	}

	.content>button {
		width: 80%;
		line-height: 40px;
		border-radius: 20px;
		background: #a2e7ff;
		margin: 40px 10%;
	}
</style>
