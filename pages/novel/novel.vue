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
            this.listId = []
            this.getList()
        },
        onReachBottom(){
            this.page = this.page + 1
            this.getList()
        },
		data() {
			return {
				list:[],
                listId:[],
                page: 1
			}
		},
		methods: {
            getList(){
                uni.startPullDownRefresh()
                uni.request({
                    url: 'https://www.apiopen.top/novelApi', //仅为示例，并非真实接口地址。
                    data: {
                        page: this.page
                    },
                    header: {
                        
                    },
                    complete: (res)=>{
                        uni.stopPullDownRefresh()
                    },
                    success: (res) => {
                        // console.log(res.data);
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
