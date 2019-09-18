<template>
	<view class='joke_container'>
        <view class="joke_container_hot">
            <view class="joke_container_hot_title">
                <text>最热评论</text>
            </view>
            <view class="emptyText" v-if="hotList.length===0">
                <text>[ 无 ]</text>
            </view>
            <view v-for="(item,index) in hotList" :key="item.index" class="joke_container_hot_list" v-else>
                <view class="joke_container_content">
                    <text>{{item.content}}</text>
                </view>
                <view class="joke_container_footer">
                    <view class="joke_container_footer_name nowrap">
                        <text>@{{item.user.username}}</text>
                    </view>
                    <view class="joke_container_footer_time">
                        <text>{{item.ctime.replace("T"," ")}}</text>
                    </view>
                </view>
            </view>
        </view>
        <view class="joke_container_normal">
            <view class="joke_container_normal_title">
                <text>最新评论</text>
            </view>
            <view class="emptyText" v-if="normalList.length===0">
                <text>[ 无 ]</text>
            </view>
            <view v-for="(item,index) in normalList" :key="item.index" class="joke_container_normal_list" v-else>
                <view class="joke_container_content">
                    <text>{{item.content}}</text>
                </view>
                <view class="joke_container_footer">
                    <view class="joke_container_footer_name nowrap">
                        <text>@{{item.user.username}}</text>
                    </view>
                    <view class="joke_container_footer_time">
                        <text>{{item.ctime.replace("T"," ")}}</text>
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
            // console.log(options)
            this.id = options.id
          uni.request({
              url: 'https://www.apiopen.top/satinCommentApi', //仅为示例，并非真实接口地址。
              data: {
                  id: options.id,
                  page: this.page
              },
              header: {
                  // 'custom-header': 'hello' //自定义请求头信息
              },
              success: (res) => {
                  // console.log(res.data);
                  uni.hideLoading();
                  if(res.data.code===200){
                      this.hotList = res.data.data.hot.list
                      this.normalList = res.data.data.normal.list
                  }
                  
              }
          });  
        },
		data() {
			return {
                id:null,
				page:1,
                hotList:[],
                normalList:[]
			}
		},
		methods: {
			
		}
	}
</script>

<style>
    .joke_container{
        
    }
    .joke_container_hot,
    .joke_container_normal{
        /* box-shadow: 0 1px 2px #aaa; */
        padding:5px 10px;
        margin:5px 0 10px;
    }
    .joke_container_hot_title,
    .joke_container_normal_title{
        font-size: 16px;
        font-weight: bold;
        padding: 8px 0 10px;
    }
    .joke_container_hot_list,
    .joke_container_normal_list{
        padding: 8px 0px;
        margin: 0 10px;
        border-bottom: 1px solid #eee;
    }
    .joke_container_content{
        font-size: 14px;
    }
    .joke_container_footer{
        display: flex;
        justify-content: space-between;
        margin-top: 8px;
        
        color: #aaa;
        font-size: 12px;  
    }
    
    .joke_container_footer image{
        width: 22px;
        height: 22px;
        margin-left: 8px;
    }
    .joke_container_footer_time{
        max-width: 180px;
    }
</style>
