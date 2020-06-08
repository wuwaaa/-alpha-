<template>
	<view>
		<template v-if="searchList.length === 0">
			<!--搜索历史-->
			<view class="py-2 font-md px-2">搜索历史</view>
			<view class="flex flex-wrap">
				<view class="border rounded font mx-2 my-1 px-2" 
				v-for="(item,index) in list" :key="index"
				hover-class="bg-light"
				@click="clickSearchHistory(item)">{{item}}</view>
			</view>
		</template>
		<template v-else>
			<!--数据列表-->
			<block v-for="(item,index) in searchList" :key="index">
				<common-list :item="item" :index="index"></common-list>
			</block>
		</template>
	</view>
</template>

<script>
	//测试数据
	const demo =[
		{
			username:"昵称",//昵称
			userpic:"/static/girl.png",//用户头像
			newstime:"2020-5-1 下午 09：23",//发帖时间
			isFollow:false,//是否关注
			title:"我是标题",
			titlepic:"/static/test.jpg",
			support:{
				type:"support",
				support_count:1,
				unsupport_count:2,
				
			},
			comment_count:3,//评论数
			share_num:2//分享人数
		
		},
		{
			username:"昵称",//昵称
			userpic:"/static/girl.png",//用户头像
			newstime:"2020-5-1 下午 09：23",//发帖时间
			isFollow:false,//是否关注
			title:"我是标题",
			titlepic:"",
			support:{
				type:"unsupport",
				support_count:1,
				unsupport_count:2,
				
			},
			comment_count:3,//评论数
			share_num:2//分享人数
		
		},
		{
			username:"昵称",//昵称
			userpic:"/static/girl.png",//用户头像
			newstime:"2020-5-1 下午 09：23",//发帖时间
			isFollow:false,//是否关注
			title:"我是标题",
			titlepic:"",
			support:{
				type:"",
				support_count:1,
				unsupport_count:2,
				
			},
			comment_count:0,//评论数
			share_num:0//分享人数
		
		}
		
	];
	import commonList from '@/components/common/common-list.vue';
	export default {
		components:{
			commonList
		},
		data() {
			return {
				searchText:"",
				list:['波斯猫','二哈拆家','幼猫要喂什么','猫咪迷惑行为大赏','金渐层','救助'],
				//搜索结果
				searchList:[]
			}
		},
		//监听导航输入
		onNavigationBarSearchInputChanged(e){
			this.searchText = e.text
		},
		//监听点击导航搜索按钮
		onNavigationBarButtonTap(e){
			if(e.index === 0){
				this.searchEvent()
			}
		},
		methods: {
			//点击搜索历史
			clickSearchHistory(text){
				this.searchText = text
				this.searchEvent()
			},
			//搜索事件
			searchEvent(){
				//收起键盘
				uni.hideKeyboard()
				//显示loading状态
				uni.showLoading({
					title:'加载中...',
					mask:false
				})
				//请求搜索
				setTimeout(()=>{
					this.searchList = demo
					//隐藏loading
					uni.hideLoading()
				},3000)
			}
		}
	}
</script>

<style>

</style>
