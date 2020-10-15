<template>
	
	<view class="record__">
		<view class="">
			<input type="text" placeholder="请输入要记录的批次号" v-model="num" />
		</view>
		<button @click="jump()">记录</button>
		
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
						key: 'num_',
						data: this.num,
					})
					console.log(this.num)
					// 跳转扫码页
					uni.navigateTo({
						url: '/pages/city/city'
					})
				}
	
	
	
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

	.record__ {
		width: 100%;
		height: 100%;
		background: #a2e7ff;
		text-align: center;
	}

	.record__>view {
		padding-top: 60%;
	}

	.record__ input {
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

