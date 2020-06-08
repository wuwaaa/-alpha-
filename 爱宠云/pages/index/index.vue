<template>
	<view>
		<!--顶部选项卡-->
		<scroll-view scroll-x :scroll-into-view="scrollInto" scroll-with-animation
		class="scroll-row border-bottom border-light-secondary"
		style="height: 100rpx;">
			<view v-for="(item,index) in tabBars" :key="index"
			class="scroll-row-item px-3 py-2 font-md" :id="'tab'+index"
			:class="tabIndex === index?'text-main font-lg font-weight-bold':''"
			@click="changeTab(index)">{{item.name}}</view>
		</scroll-view>
		
		<swiper :duration="150" :current="tabIndex" @change="onChangeTab"
		:style="'height:'+scrollH+'px;'">
			<swiper-item v-for="(item,index) in newsList" :key="index">
				<scroll-view scroll-y="true" :style="'height:'+scrollH+'px;'"
				@scrolltolower="loadmore(index)">
					<template v-if="item.list.length > 0">
					<!--列表-->
					<block v-for="(item2,index2) in item.list" :key="index2">
						<!--列表样式-->
						<common-list :item="item2" :index="index2" @follow="follow" @doSupport="doSupport"></common-list>
						<!--全局分割线-->
						<divider></divider>
					</block>
					<!--上拉加载-->
					<load-more :loadmore="item.loadmore"></load-more>
					</template>
					<!--无数据-->
					<template v-else>
						<no-thing></no-thing>
					</template>
				</scroll-view>
			</swiper-item>
		</swiper>
		
	</view>
</template>

<script>
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
	import commonList from '@/components/common/common-list.vue';// @/ 代表从根目录开始
	import loadMore from '@/components/common/load-more.vue';
	export default {
		components:{
			commonList,
			loadMore
		},
		data() {
			return {
				//列表高度
				scrollH:600,
				//顶部选项卡
				scrollInto:"",
				tabIndex:0,
				tabBars:[{
					name:"关注"
				},
				{
					name:"推荐"
				},
				{
					name:"铲屎交流地"
				},
				{
					name:"宠物救助"
				},
				{
					name:"宠物领养"
				},
				{
					name:"饲养手册"
				}],
				newsList:[]
			}
		},
		//监听点击导航栏搜索框
		onNavigationBarSearchInputClicked() {
			uni.navigateTo({
				url: '../search/search',
			})
		},
		//监听导航按钮点击事件
		onNavigationBarButtonTap(){
			uni.navigateTo({
				url: '../add-input/add-input',
			})
		},
		onLoad() {
			uni.getSystemInfo({
				success: res => {
					this.scrollH = res.windowHeight - uni.upx2px(101)
				}
			})
			//根据选项生成列表
			this.getData()

		},
		methods: {
			//获取数据
			getData(){
				var arr=[]
				for (let i = 0; i < this.tabBars.length; i++) {
					//生成列表模版
					let obj = {
						//1.上拉加载更多2.加载中...3.没有更多了
						loadmore:"上拉加载更多",
						list:[]
					}
					if (i < 2){
						obj.list=demo
					}
					arr.push(obj)
				}
				this.newsList=arr
			},
			//监听滑动
			onChangeTab(e){
				this.changeTab(e.detail.current)
			},
			//切换选项
			changeTab(index){
				if(this.tabIndex === index){
					return;
				}
				this.tabIndex=index
				//滚动到指定元素
				this.scrollInto='tab'+index
			},
			//关注
			follow(e){
				this.list[e].isFollow=true
				uni.showToast({title:'关注成功'})
			},
			//顶踩操作
			doSupport(e){
				//拿到当前对象
				let item = this.list[e.index]
				let msg = e.type === 'support' ? '顶' : '踩'
				//判断之前是否已经顶踩,若无
				if(item.support.type === ''){
					item.support[e.type+'_count']++
				}else if(item.support.type === 'support' && e.type === 'unsupport'){
					//之前顶了
					item.support.support_count--;
					item.support.unsupport_count++;
				}else if(item.support.type === 'unsupport' && e.type === 'support'){
					//之前踩了
					item.support.support_count++;
					item.support.unsupport_count--;
				}
				item.support.type = e.type
				uni.showToast({title: msg + '成功'});
			},
			//上拉加载更多
			loadmore(index){
				let item = this.newsList[index]
				//判断是否处于可加载状态
				if(item.loadmore !== '上拉加载更多') return;
				//修改当前列表的加载状态
				item.loadmore = '加载中...'
				//模拟数据请求
				setTimeout(()=>{
					//加载数据
					item.list = [...item.list,...item.list]
					//恢复加载状态
					item.loadmore = '上拉加载更多'
				},1000)
			}

		}
	}
</script>

<style>
	
</style>
