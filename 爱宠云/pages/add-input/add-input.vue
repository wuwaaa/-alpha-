<template>
	<view>
		<!--自定义导航-->
		<uni-nav-bar left-icon="back" statusBar :border="false" @clickLeft ="goBack"><!--视频中是click-left，但是该版本的uni-nav-bar.vue文件不同-->
			<view class="flex align-center justify-center w-100 font">
				所有人可见<text class="iconfont icon-tubiao_shezhi"></text><!--同理图标库改了icon-tubiao_shezhi也要变化-->
			</view>
		</uni-nav-bar>
		<!-- 文本域 -->
		<textarea v-model="content" placeholder="说一句话吧~" class="uni-textarea px-2"/>
		<!-- 多图上传 -->
		<upload-image :show="show" ref="uploadImage" :list="imageList" @change="changeImage"></upload-image>
		<!-- 底部操作条 -->
		<view class="fixed-bottom bg-white flex align-center" style="height: 85rpx;">
			<!--同理图标库改了icon-tubiao_shezhi也要变化,以及动画效果-->
			<view class="iconfont icon-caidan footer-btn animated" hover-class="bounce"></view>
			<view class="iconfont icon-huati footer-btn animated" hover-class="bounce"></view><!--同理图标库改了icon-tubiao_shezhi也要变化-->
			<view class="iconfont icon-tupian footer-btn animated" hover-class="bounce"
			@click="iconClickEvent('uploadImage')"></view><!--同理图标库改了icon-tubiao_shezhi也要变化-->
			
			<view class="bg-main text-white ml-auto flex justify-center align-center rounded mr-2 animated" 
			hover-class="rubberBand" style="width: 140rpx;height: 60rpx;">发送</view>
		</view>
	</view>
</template>

<script>
	import uniNavBar from '@/components/uni-ui/uni-nav-bar/uni-nav-bar.vue';
	import uploadImage from '@/components/common/upload-image.vue';
	export default {
		components:{
			uniNavBar,
			uploadImage
		},
		data() {
			return {
				content:"",
				imageList:[],
				//是否已经弹出提示框
				showBack:false
			}
		},
		computed:{
			show(){
				return this.imageList.length > 0
			}
		},
		//监听返回
		onBackPress(){
			if((this.content !== '' || this.imageList.length >0) && !this.showBack){
				uni.showModal({
					content:'是否要保存为草稿？',
					showCancel:true,
					cancelText:'不保存',
					confirmText:'保存',
					success: res =>{
						if(res.confirm){
							this.store()
						} else{//点击取消，清除缓存
							uni.removeStorage({
								key:"add-input"
							})
						}
						//手动执行返回
						uni.navigateBack({delta:1});
					}
				});
				this.showBack = true
				return true
			}
		},
		//页面加载时
		onLoad(){
			uni.getStorage({
				key:"add-input",
				success:(res)=>{
					if(res.data){
						let result = JSON.parse(res.data)
						/* console.log(result); 奇怪，这里能取到图片，但是教程取不到*/
						this.content =result.content
						this.imageList = result.imageList
					}
				}
			})
		},
		methods: {
			//底部图片点击事件
			iconClickEvent(e){
				switch(e){
					case 'uploadImage':
					this.$refs.uploadImage.chooseImage()
						break;
				}
			},
			//返回上一步
			goBack(){
				uni.navigateBack({delta:1});
			},
			//选中图片
			changeImage(e){
				this.imageList = e
			},
			//保存操作
			store(){
				//保存为本地存储
				let obj={
					content:this.content,
					imageList:this.imageList
				}
				uni.setStorage({
					key:'add-input',
					data:JSON.stringify(obj)
				})
			}
		}
	}
</script>

<style>
.footer-btn{
	width: 86rpx;
	height: 86rpx;
	display: flex;
	justify-content: center;
	align-content: center;
	font-size: 50rpx;
}
</style>
