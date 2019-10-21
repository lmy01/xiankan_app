<template>
	<view class="detail_container">
		<view  class="book_detail_container">
		    <view class="book_img_container">
		        <image :src="detail.cover" style="width: 120px;height: 160px;" mode=""></image>
		    </view>
		    <view class="book_introduction_container" v-show="JSON.stringify(detail)!=='{}'">
		        <view class="book_name nowrap_one">
		            <text>{{detail.title?detail.title:''}}</text>
		        </view>
		        <view class="book_author nowrap_one">
                    <text>{{detail.author?detail.author:''}}</text>
		        </view>
		        <view class="book_tag nowrap_one">
		            <text>{{detail.tags?detail.tags.replace(/,/g,'/ '):''}}</text>
		        </view>
                <view class="book_words">
                    <text>字数: {{detail.words?detail.words:''}}</text>
                </view>
                <view class="book_chapter_collection nowrap_one">
                    <navigator :url="'../novel-chapter/novel-chapter?name='+detail.title+'&total='+pageDetail.total"  hover-class="navigator-hover">
                        <button type="default" size="mini" style="margin-right: 5px;">进入章节</button>
                    </navigator>
                    <view class="book_collection" @tap="collect">
                        <image :src="bidList.indexOf(detail.bid)<0?'../../static/imgs/collection.png':'../../static/imgs/collection-selected.png'" mode=""></image>
                    </view>
                </view>                
		    </view>
		</view>
        <view  class="book_detail_container">
            {{detail.desc?detail.desc:''}}
        </view>
        <view class="recommand_container">
            <view class="recommand_title">
                <text>相关推荐</text>
            </view>
            <view class="recommand_list_container">
                <view class="recommand_list" v-for="(item,index) in recommandList" :key="index" v-if="index!==0">
                    <image :src="item.cover" mode="" style="width: 120px;height: 160px;"></image>
                    <view class="recommand_list_title nowrap_two">
                        <text>{{item.title}}</text>
                    </view>
                </view>
            </view>
        </view>
	</view>
</template>

<script>
	export default {
        onLoad(options) {
            uni.showLoading({
                title: '加载中...',
                mask: false
            });
            uni.request({
                url: 'https://www.apiopen.top/novelInfoApi',
                data: {
                    name: options.name
                },
                complete: (res)=>{
                    uni.hideLoading();
                },
                success: (res) => {
                    // console.log(res.data.data.aladdin)
                    if(res.data.code===200){
                        this.detail = res.data.data.aladdin
                        this.pageDetail = res.data.data.info
                        this.recommandList = res.data.data.data
                    }
                }
            })
        },
		data() {
			return {
				detail:{},
                pageDetail:{},
                bidList:[],
                recommandList:[]
			}
		},
		methods: {
			collect(){
			    // console.log(this.detail.bid)
			    if(this.bidList.indexOf(this.detail.bid)<0){
                    this.bidList.push(this.detail.bid)
                    uni.showToast({
                        title:'收藏成功！'
                    })
                }else{
                    this.bidList.splice(this.bidList.indexOf(this.detail.bid),1)
                    uni.showToast({
                        title:'取消收藏！'
                    })
                }
			},
		}
	}
</script>

<style>
    .book_detail_container{
        font-size: 14px;
        display: flex;
        flex-direction: row;
        padding: 5px;
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
    .book_author{
        color: #666;
        font-size: 14px;
        display: flex;
        justify-content: space-between;
        padding-bottom: 5px;
    }
    .book_tag{
        color: #aaa;
        font-size: 12px;
        padding-bottom: 8px;
    }
    .book_chapter_collection{
        display: flex;
        justify-content: space-between;
    }
    .book_words{
        font-size: 12px;
        display: flex;
        justify-content: space-between;
        padding-bottom: 10px;
    }
    .book_collection{
        display: flex;
    }
    .book_collection image{
        width: 26px;
        height: 26px;
    }
    .recommand_container{
        margin-top: 15px;
        margin-bottom: 30px;
        padding: 5px;
        border-top: 1px solid #ddd;
    }
    .recommand_title{
        font-size: 16px;
        font-weight: bold;
        padding-bottom: 5px;
    }
    .recommand_list_container{
        display: flex;
        flex-wrap: wrap;
        /* overflow: scroll; */
    }
    .recommand_list{
        display: flex;
        flex-direction: column;
        flex-wrap: wrap;
        align-content: center;
        justify-content: center;
        padding: 3px;
    }
    .recommand_list_title{
        width: 120px;
        font-size: 14px;
        font-weight: bold;
        text-align: center;
    }
</style>
