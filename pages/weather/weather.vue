<template>
	<view class="content" id="weather_wrap">
		<view class="weather_search">
		    <input class="uni-input" focus confirm-type="search" placeholder="请输入城市名" value="上海" @input="getValue" />
            <button type="default" @tap="search">搜索</button>
		</view>
        <view class="weather_result_container">
            <view class="emptyText" v-if="result.forecast.length<1">
                [无数据]
            </view>
            <view class="weather_result" v-for="(item,index) in result.forecast" v-else>
                <view class="weather_result_item">
                    {{item.date}}
                </view>
                <view class="weather_result_item">
                    {{item.type}}
                </view>
                <view class="weather_result_item">
                    {{item.high.split(' ')[1] + ' / ' + item.low.split(' ')[1]}}
                </view>
                <view class="weather_result_item">
                    {{item.fengxiang + ' ' + item.fengli.slice(9,item.fengli.length-3)}}
                </view>
            </view>
        </view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				city: '上海',
                resultFlag: false,
                result: {
                    forecast:[]
                }
			}
		},
		methods: {
            getValue(event){
                this.city = event.target.value
            },
			search(){
                console.log(this.city)
                uni.showLoading({
                    title: '正在搜索...'
                })
                uni.request({
                    url: 'https://www.apiopen.top/weatherApi',
                    data: {
                        city: this.city,
                        // key: '838020829af6b7ff274971866201c3c5'
                    },
                    success: (res) => {
                        uni.hideLoading()
                        if(res.data.code===200){
                            this.result = res.data.data
                            this.resultFlag = true
                        }
                            
                    }
                })
            }
		}
	}
</script>

<style>
    .uni-input{
        box-shadow: -1px -1px 2px inset #ccc;
        padding: 8px;
        margin-bottom: 10px;
    }
    .weather_result_container{
        padding-top: 10px;
    }
    .weather_result{
        display: flex;
    }
    .weather_result_item{
        min-width: 30px;
        font-size: 14px;
        padding: 10px;
    }
</style>
