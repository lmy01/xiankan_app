<template>
	<view class="content" id="inquire_wrap">
		<view v-for="(item,index) in menuList" :key="index" class="inquire_list" @tap="select(item)">
            <image :src="item.img" mode=""></image>
		    {{item.title}}
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				menuList: [
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
                        title: '字体转换',
                        img: '../../static/imgs/transfer.png',
                        url: ''
                    }
                    
                ]
			}
		},
		methods: {
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
    #inquire_wrap{
        display: flex;
        flex-direction: row;
        flex-wrap: wrap;
        justify-content: center;
        align-content: center;
    }
    .inquire_list{
        width: 50%;
        height: 70px;
        line-height: 70px;
        font-size: 16px;
        font-weight: bold;
        text-align: center;
        padding: 10px;
        margin: 10px;
        box-shadow: 0 2px 2px #ccc;
    }
    .inquire_list image{
        width: 40px;
        height: 40px;
        margin-right: 10px;
        vertical-align: middle;
    }
</style>
