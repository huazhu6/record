<template>
	<view class="son_">
		<view v-for="i in goods.data.childLabel" :key='i' class="list">
			<image :src="i.product.small_image" mode=""></image>
			<view class="title">{{i.product.product_name}}</view>
			<view class="num">X {{i.labels.length}}</view>
			<button @click="dell(i.labels)">删除</button>
		</view>
		<!-- 全部删除 -->
		<button type="default" @click="out()">删除清空</button>
	</view>
</template>

<script>
	export default{
		data(){
			return{
				goods:'',  //存放商品
				token:'',
				qr:[],   //存放要删除的子节点
			}
		},
		onShow() {
			var this_=this
			uni.getStorage({
				key: 'goods',
				success(e) {
					console.log(e);
					this_.goods=e.data
					this_.go()
					this_.empty()
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
		methods:{
			// 如果没有子节点，提醒用户没有，并返回上一级菜单
			go(){
				if(JSON.stringify(this.goods.data.childLabel)=='{}'){
					// 提醒用户该标签下没有
					uni.showToast({
						title: '该标签下没有子标签',
						icon: 'none'
					})
					// 返回菜单页 延迟2秒执行
					setTimeout(function() {
						uni.navigateBack({
							delta: 1
						})
					}, 1500)
				}
			},
			// 删除节点
			dell(i){
				var this_ = this
				var token_ = 'Token ' + this_.token
				uni.request({
					url: "https://test.gotrack.com.cn/api/updateLabelRel/",
					data: {
						"childLabels": i, //子节点
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
									delta: 1
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
			},
		    // 遍历所有节点删除
		    empty(){
		    	
		    	for(var i in this.goods.data.childLabel){
		    		this.goods.data.childLabel[i].labels.forEach(item=>{
		    			this.qr.push(item)
		    		})
		    	}
		    	
		    },
			// 删除节点
			out(){
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
							// 返回菜单页 延迟1.5秒执行
							setTimeout(function() {
								uni.navigateBack({
									delta: 1
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
	page{
		height: 100%;
	}
	.son_{
		position: relative;
		height: 100%;
	}
	.son_>.list{
		display: flex;
		margin: 10px;
		box-shadow: 0 0 5px #999999;
		padding: 5px;
		border-radius: 10px;
	}
	.son_>.list>image{
		width: 200px;
		height: 100px;
	}
	.son_>.list>.title{
		overflow: hidden;
		white-space: nowrap;
		text-overflow: ellipsis;
		line-height: 100px;
	}
	.son_>.list>.num{
		line-height: 100px;
		width: 80px;
	}
	.son_>button{
		position: absolute;
		left: 0;
		bottom: 20px;
		z-index: 8;
		width: 100%;
	}
</style>
