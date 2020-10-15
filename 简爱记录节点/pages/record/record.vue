<template>
	<view class="record">
		<view class="">
			<input type="text" placeholder="请输入批次号" v-model="num" />
		</view>
		<button @click="jump()">录入</button>

	</view>
</template>

<script>
	export default {
		data() {
			return {
				num: '' //记录批次号
			}
		},

		methods: {
			// 跳转扫码页
			jump() {
				if (this.num == '') {
					// 未输入提醒输入
					uni.showToast({
						title: '请输入批次号',
						// icon: 'none'
						// icon:'loading'
					});
				} else {
					// 记录批次号节点
					uni.setStorage({
						key: 'num',
						data: this.num,
					})
					console.log(this.num)
					// 跳转扫码页
					uni.navigateTo({
						url: '/pages/QR/QR'
					})
				}



			},
			// 记录节点
			node() {

				uni.request({
					url: "https://test.gotrack.com.cn/api/provenance-node/",
					data: {
						'label': ['https://www.gotrack.com.cn/qr?id=bc03ba-1596444221-656304'],
						'location': '日本大阪府大阪市北区堂島浜2-1-40,大阪市,日本',
						'latitude': 34.680612,
						'longitude': 135.501383,
						'label_type': 1,
						'extra_info': {
							'batchNumber': 'FHG-1515:15DJIL'
						}
					},
					header: {
						'Authorization': 'Token 55aeb5b9443b2dca57852722e74179f4f4615147'
					},
					method: 'POST',
					success: res => {
						console.log(res)
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

	.record {
		width: 100%;
		height: 100%;
		background: #a2e7ff;
		text-align: center;
	}

	.record>view {
		padding-top: 60%;
	}

	.record input {
		width: 80%;
		height: 50px;
		border-radius: 30px;
		border: 1px solid #333333;
		margin: 0 10%;
		background: #FFFFFF;
	}

	button {
		width: 80%;
		height: 50px;
		border-radius: 30px;
		background: #FFFFFF;
		margin-top: 5%;
	}
</style>
