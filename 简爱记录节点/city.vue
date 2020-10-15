<template>
	<view class="city">
		<view>城市选择</view>
		<input type="text" placeholder="请输入国家" v-model="country" />
		<input type="text" placeholder="请输入省份" v-model="province" />
		<input type="text" placeholder="请输入城市" v-model="city" />
		<button type="default" @click="node()">记录</button>
	</view>
</template>

<script>
	export default{
		data(){
			return{
				token:'',  //保存用户token
				country:'',   //保存国家
				province:'',   //保存省
				city:'',      //保存城市
				num:'',       //保存批次号
			}
		},
		onShow() {
			var this_=this
			// 获取token
			uni.getStorage({
				key: 'token',
				success(e) {
					console.log(e)
					// this_.token = 'token '+e.data
					this_.token = 'Token '+ e.data
				}
			});
			
			uni.getStorage({
				key: 'num_',
				success(e) {
					console.log(e)
					this_.num=e.data
				}
			});
			
		},
		methods:{
			// 记录节点
			node(){
				var this_=this
				
				console.log(this_.country+'.'+this_.province+'.'+this_.city)
				
				uni.request({
					url:'https://test.gotrack.com.cn/api/record-position/',
					data:{
						'customer_order': this_.num,
						'destination': '中国.重庆.江北区'
					},
					header: {
						'Authorization': this_.token
					},
					method: 'POST',
					success:res=>{
						console.log(res)
					}
				})
			}
			
			
		}
	}
</script>

<style>
	.city{
		text-align: center;
		margin-top: 60%;
	}
</style>
