<template>
	<view class="content" id="video_wrap">
		<view v-for="(item,index) in list" :key="index" class="video_list_container">
		    <view class="video_content">
		        <video :src="item.video" class="video_content_video" controls :poster="item.header" objectFit="cover">
                    <view class="video_content_avater_name">
                        <image :src="item.thumbnail" mode="" style="width: 28px;height: 28px;border-radius: 50%;"></image>
                        <text class="video_content_name">@{{item.username}}</text>
                    </view>
                </video>
		    </view>
            <view class="video_content_text_collect">
                <view class="video_content_text">
                    <text class="nowrap_two">{{item.text}}</text>
                </view>
                <view class="video_content_collect">
                    <image v-if="soureidList.indexOf(item.soureid)<0" src="../../static/imgs/collection.png" mode="" @tap="collect(item)"></image>
                    <image v-else src="../../static/imgs/collection-selected.png" mode="" @tap="collect(item)"></image>
                    <image src="../../static/imgs/share.png" mode="" @tap="share(item)"></image>
                </view>
            </view>
		</view>
	</view>
</template>

<script>
	export default {
        onLoad() {
            this.list = []
            this.listId = []
            this.getList()
        },
        onReachBottom(){
            this.page = this.page + 1
            this.getList()
        },
		data() {
			return {
				list: [],
                listId:[],
                page: 1,
                soureidList:[],
			}
		},
		methods: {
			getList(){
                uni.startPullDownRefresh()
                uni.request({
                    url:'https://www.apiopen.top/satinGodApi',
                    data: {
                        type: '5',
                        page: this.page
                    },
                    complete: (res)=>{
                        uni.stopPullDownRefresh()
                    },
                    success: (res) => {
                        if(res.data.code===200){
                            let newList = []
                            let newIdList = []
                            newList = res.data.data.filter(item=>{
                                if(this.listId.indexOf(item.bid)<0){
                                    return item
                                }
                            })
                            newIdList = newList.map(item=>item.bid)
                            
                            this.list.push(...newList);
                            this.listId.push(...newIdList);
                        }
                        
                    }
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
    .video_list_container{
        margin-bottom: 15px;
        box-shadow: 0 0 3px #ccc;
    }
    .video_content{
        display: flex;
        flex-direction: column;
        align-content: stretch;
    }
    .video_content_video{
        width: 100%;
    }
    .video_content_avater_name{
        display: flex;
        min-height: 30px;
        color: #fff;
        padding: 10px 10px 0;
    }
    .video_content_name{
        padding-top: 12px;
        margin-left: 5px;
    }
    .video_content_text_collect{
        display: flex;
        flex-direction: row;
        justify-content: space-between;
        padding: 8px 5px 12px;
    }
    .video_content_text{
        font-size: 16px;
        font-weight: bold;
    }
    .video_content_collect{
        min-width: 50px;
        padding-left: 10px;
    }
    .video_content_collect image{
        width: 24px;
        height: 24px;
    }
</style>
