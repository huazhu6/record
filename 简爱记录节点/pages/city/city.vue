<template>
	<view>
		<!-- 浮窗 -->
		<view class="QR">
			<!-- 浮窗 -->
			<view>
				<image src="../../static/QR.png" mode=""></image>
				<view>扫描二维码，进行查询</view>
				<button @click="scan()">扫描 <text>( {{num_}} )</text></button>
			</view>
		</view>




		<view @tap="togglePopup('bottom','popup')" style="padding: 40upx;display: flex;align-items: center;">
			<view>中国—</view>

			<view v-for="(item, index) in selectList" :key="index">
				{{item.txt}}<span v-show="index == 0 || index == 2">—</span>
			</view>
		</view>
		<button @click="node()" v-if='num_!==0'>提交</button>
		<uni-popup ref="popup" :type="type" @change="change">
			<view class="select-border">
				<view class="header">
					<view class="title">
						选择地区
					</view>
					<view class="cancel-icon" @tap="cancel('popup')">
						X
					</view>
				</view>
				<view class="select-box">
					<view class="select-item">
						<view class="select-list" @tap="tabEvent(index)" :class="indexTab == index ? 'selected' : ''" v-for="(item, index) in selectList"
						 :key="index">
							{{item.txt}}
						</view>
					</view>
					<view class="select-item-box">
						<!-- 省 -->
						<view class="province-box" v-show="proviceShow">
							<view class="select-list-cont" @tap="provinceEvent(item,index)" v-for="(item,index) in provinceData" :key="item.code">
								{{item.name}}<span class="check" v-show="index == checkOne">√</span>
							</view>
						</view>
						<!-- 市 -->
						<view class="city-box" v-show="cityShow">
							<view class="select-list-cont" @tap="cityEvent(item,index)" v-for="(item,index) in cityData" :key="item.code">
								{{item.name}}<span class="check" v-show="index==checkTwo">√</span>
							</view>
						</view>
						<!-- 区 -->
						<!-- <view class="area-box" v-show="areaShow">
							<view class="select-list-cont" @tap="areaEvent(item,index)" v-for="(item,index) in areaData" :key="item.code">
								{{item.name}}<span class="check" v-show="index==checkThree">√</span>
							</view>
						</view> -->
					</view>
				</view>
			</view>
		</uni-popup>
	</view>
</template>

<script>
	import cityDatas from '../../common/city.area.js'
	import uniPopup from './uni-pop/uni-pop.vue'
	export default {
		components: {
			uniPopup
		},
		data() {
			return {
				place: '', //地点
				latitude: '', //坐标
				longitude: '',
				qr: [], //保存扫描数据
				num_: 0, //已经扫码次数

				provinceData: cityDatas,
				cityData: [],
				areaData: [],
				selectList: [{
					txt: '请选择'
				}, {
					txt: '请选择'
				}],
				tabOne: '请选择',
				indexTab: 0,
				proviceShow: true,
				areaShow: false,
				cityShow: false,
				show: false,
				type: '',
				checkOne: null,
				checkTwo: null,
				checkThree: null,
			}
		},
		onShow() {
			var this_ = this
			// 获取token
			uni.getStorage({
				key: 'token',
				success(e) {
					console.log(e)
					// this_.token = 'token '+e.data
					this_.token = 'Token ' + e.data
				}
			});

			uni.getStorage({
				key: 'num_',
				success(e) {
					console.log(e)
					this_.num = e.data
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
		onLoad() {

		},
		watch: {

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
				if (this.num_ == 10) {
					uni.showToast({
						title: '最多只能扫描10个节点二维码',
						icon: 'none'
					})
				} else {
					//扫描二维码
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

				}

			},

			// 记录节点
			node() {
				var this_ = this

				console.log('中国.' + this_.selectList[0].txt + '.' + this_.selectList[1].txt)
				if (this_.selectList[1].txt == '请选择') {
					uni.showToast({
						title: '请先选择城市',
						icon: 'none'
						// icon: 'loading'
					});
				} else {
					// 显示地理位置信息
					// uni.showToast({
					// 	title: this_.place,
					// 	icon: 'none'
					// });
					uni.request({
						url: 'https://test.gotrack.com.cn/api/provenance-node/',
						data: {
							'label': this_.qr, //扫描标签
							'location': this_.place,
							'latitude': this_.latitude,
							'longitude': this_.longitude,
							'label_type': 0,
							
							// 'customer_order':this_.qr,
							// 'destination':'中国.' + this_.selectList[0].txt + '.' + this_.selectList[1].txt,
							'extra_info': {
								'customer_order': this_.num,
								// 'destination':this_.place,
								'destination': '中国.' + this_.selectList[0].txt + '.' + this_.selectList[1].txt
							}

						},
						header: {
							'Authorization': this_.token
						},
						method: 'POST',
						success: res => {
							console.log(res)
							if(res.data.msg=='successful.'){
								uni.showToast({
									title: '记录成功',
									icon: 'none'
								});
							}else{
								uni.showToast({
									title: '记录失败',
									icon: 'none'
								});
							}
							
						}
					})
				}

			},

			togglePopup(type, open) {
				this.type = type
				if (open === 'tip') {
					this.show = true
				} else {
					this.$refs[open].open()
				}
			},
			cancel(type) {
				if (type === 'tip') {
					this.show = false
					return
				}
				this.$refs[type].close()
			},
			change(e) {
				if (e.show == true) {
					uni.hideTabBar()
				} else {
					uni.showTabBar()
				}
			},
			tabEvent(index) {
				this.indexTab = index
				if (this.indexTab == 0) {
					this.proviceShow = true
					this.cityShow = false
					this.areaShow = false
					// this.checkOne = null
					this.checkTwo = null
					this.checkThree = null
					// this.cityData = []
					this.areaData = []
					// this.selectList[0].txt = "请选择"
					this.selectList[1].txt = "请选择"
					// this.selectList[2].txt = "请选择"
				} else if (this.indexTab == 1) {
					this.proviceShow = false
					this.cityShow = true
					this.areaShow = false
					// this.checkTwo = null
					this.checkThree = null
					// this.areaData = []
					// this.selectList[1].txt = "请选择"
					// this.selectList[2].txt = "请选择"
				} else if (this.indexTab == 2) {
					this.proviceShow = false
					this.cityShow = false
					this.areaShow = true
				}
			},
			provinceEvent(data, index) {
				this.checkOne = index
				this.selectList[0].txt = data.name
				this.indexTab = 1
				this.proviceShow = false
				this.cityShow = true
				this.areaShow = false
				this.cityData = data.cityList
			},
			cityEvent(data, index) {
				this.checkTwo = index
				this.selectList[1].txt = data.name
				// this.indexTab = 2
				// this.proviceShow = false
				// this.cityShow = false
				// this.areaShow = true
				// this.areaData = data.areaList

				// 隐藏弹窗
				this.cancel('popup')
			},
			areaEvent(data, index) {
				this.checkThree = index
				this.selectList[2].txt = data.name

				this.cancel('popup')
			}
		}
	}
</script>

<style>
	.header {
		display: flex;
		align-items: center;
		justify-content: space-between;
		padding: 35upx;
	}

	/* 扫描二维码浮窗 */
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



	.title {
		font-size: 34upx;
		font-family: PingFang SC;
		font-weight: bold;
		color: rgba(51, 51, 51, 1);
	}

	.cancel-icon {
		font-size: 34upx;
		color: rgba(153, 153, 153, 1);
	}

	.check {
		padding-left: 17upx;
		color: #FF7E28;
	}

	.select-box {
		height: 1024upx;
	}

	.select-item {
		display: flex;
		align-items: center;
		padding-left: 50upx;
		margin-bottom: 20upx;
		border-bottom: 1px solid #F6F6F6;
	}

	.select-list {
		width: 120upx;
		height: 40upx;
		text-align: center;
		overflow: hidden;
		/*超出部分隐藏*/
		text-overflow: ellipsis;
		/* 超出部分显示省略号 */
		white-space: nowrap;
		/*规定段落中的文本不进行换行 */
		font-size: 30upx;
		font-family: PingFang SC;
		font-weight: bold;
		color: rgba(51, 51, 51, 1);
		margin-right: 30upx;
		border-bottom: 1px solid #FFFFFF;
	}

	.select-list-cont {
		padding-left: 67upx;
		font-size: 30upx;
		font-family: PingFang SC;
		font-weight: 500;
		color: rgba(51, 51, 51, 1);
		line-height: 40px;
	}

	.selected {
		border-bottom: 1px solid #F0AD4E;
		color: rgba(255, 133, 0, 1);
	}
</style>
