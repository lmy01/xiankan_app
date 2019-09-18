<template>
	<view class="content">
		<view v-for="(item,index) in list" :key="index" class="book_list_container" @tap="getDetail(item)">
		    <view class="book_img_container">
		        <image :src="item.book_cover" style="width: 120px;height: 160px;" mode=""></image>
		    </view>
            <view class="book_introduction_container">
                <view class="book_name nowrap_one">
                    <text>{{item.bookname}}</text>
                </view>
                <view class="book_author_new_container nowrap_one">
                    <view class="book_author">
                        <text>{{item.author_name}}</text>
                    </view>
                    <view class="book_new">
                        <text>{{item.is_new===1?'新':''}}</text>
                    </view>
                </view>
                <view class="book_tag nowrap_one">
                    <text>{{item.tag?item.tag.join("/ "):''}}</text>
                </view>
                <view class="book_info nowrap_four">
                    <text>{{item.book_info}}</text>
                </view>
            </view>
		</view>
	</view>
</template>

<script>
	export default {
        onLoad() {
            this.list = []
            this.getList()
        },
		data() {
			return {
				list:[],
                page: 1
			}
		},
		methods: {
            getList(){
                uni.showLoading({
                    title: '正在加载中...',
                    mask: false
                });
                uni.request({
                    url: 'https://www.apiopen.top/novelApi', //仅为示例，并非真实接口地址。
                    data: {
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
            getDetail(item){
                uni.navigateTo({
                    url:'../novel-detail/novel-detail?name=' + item.bookname
                })
            }
        }
	}
</script>

<style>
    .book_list_container{
        display: flex;
        flex-direction: row;
        flex-grow: 1;
        padding: 5px;
        margin: 10px 0;
        box-shadow: 0 2px 2px #dfdfdf;
    }
    .book_img_container{
        
    }
    .book_introduction_container{
        padding-left: 8px;
    }
    .book_name{
        font-weight: bold;
        font-size: 16px;
        padding-bottom: 3px;
    }
    .book_author_new_container{
        color: #666;
        font-size: 14px;
        display: flex;
        flex-direction: row;
        justify-content: space-between;
    }
    .book_new{
        color: #fff;
        font-size: 13px;
        background-color: #F5B800;
        padding:0 3px;
        margin-right: 8px;
    }
    .book_tag{
        color: #aaa;
        font-size: 12px;
    }
    .book_info{
        font-size: 14px;
        padding-top: 5px;
    }
</style>
