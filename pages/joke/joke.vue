<template>
	<view class="content" id="joke_wrap">
		<view v-for="(item,index) in list" :key="index" class="joke_container">
            <view class="joke_container_header">
                <view class="joke_container_header_name">
                    <text>@{{item.username}}</text>
                </view>
                <view>
                    <image v-if="soureidList.indexOf(item.soureid)<0" src="../../static/imgs/collection.png" mode="" @tap="collect(item)"></image>
                    <image v-else src="../../static/imgs/collection-selected.png" mode="" @tap="collect(item)"></image>
                    <image src="../../static/imgs/share.png" mode="" @tap="share(item)"></image>
                </view>
            </view>
            <view class="joke_container_content" @tap="openDetail(item)">
                <text>{{item.text}}</text>
            </view>
            <view class="joke_container_footer">
                <view class="joke_container_footer_time">
                    <text>{{item.passtime}}</text>
                </view>
            </view>
        </view>
        <!-- <uni-load-more></uni-load-more> -->
    </view>
</template>

<script>
    // import {uniLoadMore} from "@dcloudio/uni-ui"
	export default {
        onLoad() {
           this.list = []
           this.getList()
        },
        onPullDownRefresh(){
            console.log("下拉")
        },
        onReachBottom(){
            this.page = this.page + 1
            this.getList()
        },
        // components: {uniLoadMore},
		data() {
			return {
				list:[],
                soureidList:[],
                page:1,
			}
		},
		methods: {
			getList(){
                uni.showLoading({
                    title: '正在加载中...',
                    mask: false
                });
                uni.request({
                    url: 'https://www.apiopen.top/satinGodApi', //仅为示例，并非真实接口地址。
                    data: {
                        type: '2',
                        page: this.page
                    },
                    header: {
                        // 'custom-header': 'hello' //自定义请求头信息
                    },
                    success: (res) => {
                        // console.log(res.data);
                        uni.hideLoading();
                        if(res.data.code===200){
                            this.list.push(...res.data.data);
                        }
                    }
                });
            },
            openDetail(item){
                uni.navigateTo({
                    // ? 后面加要传的参数， 多个参数用 & 隔开 
                    url: '../joke-detail/joke-detail?id='+item.soureid
                })
            },
            collect(item){
                // console.log(item.soureid)
                if(this.soureidList.indexOf(item.soureid)<0){
                    this.soureidList.push(item.soureid)
                    uni.showToast({
                        title:'收藏成功！'
                    })
                }else{
                    this.soureidList.splice(this.soureidList.indexOf(item.soureid),1)
                    uni.showToast({
                        title:'取消收藏！'
                    })
                }
            },
            share(item){
                uni.showModal({
                    content: '分享功能正在开发中...',
                    showCancel: false
                });
            }
		}
	}
</script>

<style>
    .joke_container{
        padding:5px 15px;
        margin:5px 0 10px;
        box-shadow: 0 1px 2px #aaa;
    }
    .joke_container_header{
        display: flex;
        justify-content: space-between;
        margin-bottom: 5px;
        color: #aaa;
        font-size: 13px;
    }
    .joke_container_header_name{
        max-width: 220px;
        padding: 3px 0 5px;
    }
    .joke_container_header image{
        width: 22px;
        height: 22px;
        margin-left: 8px;
    }
    .joke_container_content{
        font-size: 15px;
    }
    .joke_container_footer{
        display: flex;
        margin-top: 8px;
        color: #aaa;
        font-size: 12px;  
    }
    .joke_container_footer_time{
        
    }
    
    
</style>
