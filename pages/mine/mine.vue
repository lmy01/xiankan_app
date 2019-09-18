<template>
	<view class="content" id="user_wrap">
		<view class="user_container">
		    <image src="../../static/imgs/avater-man.png" mode="" style="width: 100px;height: 100px;border-radius: 50%;"></image>
            <view class="user_info">
                <text class="user_name">{{userInfo.name}}</text>
                <text class="user_text">{{userInfo.text}}</text>
            </view>
            <view class="user_setting">
                <image src="../../static/imgs/setting.png" mode="" style="width: 24px;height: 24px;"></image>
            </view>
		</view>
        <view class="collection_container">
            <view class="collection_title">
                <text>我的收藏</text>
            </view>
            <view class="collection_list_container">
                <view v-for="(item,index) in menuList" :key="index" class="collection_list">
                    <image :src="item.img" mode=""></image>
                    {{item.title}}
                </view>
            </view>
        </view>
        <view class="tool_container">
            <view class="tool_title">
                <text>常用工具</text>
            </view>
            <view class="tool_list_container">
                <view v-for="(item,index) in toolList" :key="index" class="tool_list" @tap="select(item)">
                    <image :src="item.img" mode=""></image>
                    {{item.title}}
                </view>
            </view>
        </view>
        <view class="exit_container">
            <button type="warn" @tap="exit">退出登录</button>
        </view>
	</view>
</template>

<script>
	export default {
        onLoad() {
            uni.showLoading({
                title: '获取信息...'
            })
            uni.request({
                url: 'https://www.apiopen.top/login?key=00d91e8e0cca2b76f515926a36db68f5&phone=13594347817&passwd=123456',
                success: (res) => {
                    // console.log(res.data.data)
                    uni.hideLoading()
                    if(res.data.code===200){
                        this.userInfo = res.data.data
                    }
                }
            })
        },
		data() {
			return {
				userInfo: {},
                menuList: [
                    {
                        title: '笑话',
                        img: '../../static/imgs/joke-selected.png',
                        url: '../joke/joke'
                    },
                    {
                        title: '小说',
                        img: '../../static/imgs/novel-selected.png',
                        url: '../novel/novel'
                    },
                    {
                        title: '视频',
                        img: '../../static/imgs/video-selected.png',
                        url: '../video/video'
                    }
                ],
                toolList: [
                    {
                        title: '天气预报',
                        img: '../../static/imgs/weather.png',
                        url: '../weather/weather'
                    },
                    {
                        title: '周公解梦',
                        img: '../../static/imgs/sleep.png',
                        url: ''
                    },
                    {
                        title: '星座运势',
                        img: '../../static/imgs/constellation.png',
                        url: ''
                    },
                    {
                        title: '号码归属',
                        img: '../../static/imgs/number-attribution.png',
                        url: ''
                    },
                    {
                        title: '邮编地址',
                        img: '../../static/imgs/zip-code.png',
                        url: ''
                    },
                    {
                        title: '成语词典',
                        img: '../../static/imgs/idiom-dictionary.png',
                        url: ''
                    }
                ]
			}
		},
		methods: {
			exit(){
                uni.showModal({
                    content: '确定退出登录？',
                    success: (res) => {
                        if(res.confirm){
                            uni.showToast({
                                title: '退出成功！'
                            })
                        }
                    }
                })
            },
            select(item){
                if(item.title==='天气预报'){
                    uni.navigateTo({
                        url: item.url
                    })
                }else{
                    uni.showModal({
                        content: '正在开发中...',
                        showCancel: false
                    });
                }
                
            }
		}
	}
</script>

<style>
    .user_container{
        padding:10px 20px;
        display: flex;
        justify-content: space-between;
        box-shadow: 0 1px 1px #eee;
    }
    .user_info{
        display: flex;
        flex-direction: column;
        justify-content: center;
    }
    .user_name{
        font-size: 18px;
        font-weight: bold;
        margin-bottom: 10px;
    }
    .user_text{
        font-size: 13px;       
    }
    .user_setting{
        display: flex;
        flex-direction: row;
    }
    .collection_container,
    .tool_container,
    .exit_container{
        margin-top:20px;
    }
    .collection_title,
    .tool_title{
        font-size: 15px;
        font-weight: bold;
        padding:5px;
        border-bottom: 1px solid #eee;
    }
    .collection_list_container,
    .tool_list_container{
        display: flex;
        flex-wrap: wrap;
    }
    .collection_list,
    .tool_list{
        width: 33.3%;
        height: 70px;
        line-height: 70px;
        font-size: 14px;
        font-weight: bold;
        text-align: center;
        /* padding: 10px; */
        box-shadow: 0 1px 1px #eee;
    }
    .collection_list image,
    .tool_list image{
        width: 24px;
        height: 24px;
        margin-right: 10px;
        vertical-align: middle;
    }
</style>
