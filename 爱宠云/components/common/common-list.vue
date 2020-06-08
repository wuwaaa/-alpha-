<template>
	<!--列表样式-->
	<view class="p-2">
		<!--头像昵称|关注按钮-->
		<view class="flex align-center justify-between">
			<view class="flex align-center">
				<!--头像-->
				<image class="rounded-circle mr-2" 
				:src="item.userpic" @click="openSpace"
				style="width: 65rpx;height: 65rpx;" 
				lazy-load></image>
				<!--昵称及发布时间-->
				<view>
					<view class="font" style="line-height: 1.5;">{{item.username}}</view>
					<text class="font-sm text-light-muted" style="line-height: 1.5;">{{item.newstime}}</text>
				</view>
			</view>
			<!--按钮-->
			<view @click="follow" v-if="!item.isFollow"
			class="flex align-center justify-center rounded bg-main text-white animated faster" 
			style="width: 90rpx;height: 50rpx;"
			hover-class="rubberBand">
				关注
			</view>
		</view>
		<!--标题-->
		<view class="font-md my-1" @click="openDetail">{{item.title}}</view>
		<!--图片-->
		<image v-if="item.titlepic" :src="item.titlepic" @click="openDetail"
		class="rounded w-100" style="height: 350rpx;"></image>
		<!--图标按钮-->
		<view class="flex align-center">
			<!--赞-->
			<view class="flex align-center justify-center flex-1 animated faster" hover-class="rubberBand text-main"
			@click="doSupport('support')"
			:class="item.support.type === 'support' ? 'support-active' : '' ">
				<!--因为animate.css是阉割版，所以并没有全部动画效果，故选了rubberBand-->
				<text class="iconfont icon-dianzan2 mr-2"></text><!--这个点赞2，如果图标库改了，这里图标名icon-dianzan2也要记得更改-->
				<text>{{item.support.support_count > 0 ? item.support.support_count : '支持'}}</text>
			</view>
			<!--踩-->
			<view class="flex align-center justify-center flex-1 animated faster" hover-class="rubberBand text-main" 
			@click="doSupport('unsupport')"
			:class="item.support.type === 'unsupport' ? 'support-active' : '' ">
				<text class="iconfont icon-cai mr-2"></text><!--也要记得更改-->
				<text>{{item.support.unsupport_count > 0 ? item.support.unsupport_count : '反对'}}</text>
			</view>
			<!--评论-->
			<view class="flex align-center justify-center flex-1 animated faster" hover-class="rubberBand text-main"
			@click="openDetail">
				<text class="iconfont icon-pinglun mr-2"></text><!--也要记得更改-->
				<text>{{item.comment_count > 0 ? item.comment_count : '评论'}}</text>
			</view>
			<!--分享-->
			<view class="flex align-center justify-center flex-1 animated faster" hover-class="rubberBand text-main"
			@click="openDetail">
				<text class="iconfont icon-fenxiang mr-2"></text><!--也要记得更改-->
				<text>{{item.share_num > 0 ? item.share_num : '分享'}}</text>
			</view>
		</view>
	</view>
	
</template>

<script>
	export default{
		props:{
			item:Object,
			index:Number
		},
		methods:{
			//打开个人空间
			openSpace(){
				console.log('打开个人空间');
			},
			//关注
			follow(){
				//通知父组件
				this.$emit('follow',this.index)
			},
			//进入详情页
			openDetail(){
				console.log('进入详情页');
			},
			//顶踩操作
			doSupport(type){
				this.$emit('doSupport',{
					type:type,
					index:this.index
				})
			}
		},
		
	}
</script>

<style>
	.support-active{
		color: #FF4A6A;
	}
</style>
