<template>
	<view class="chapter_container">
        <view class="chapter_title">
            {{name}}
            <text class="title_sub">共{{total}}章</text>
        </view>
        <view class="chapter_list_container">
            <view v-for="(item,index) in total" :key="index" class="chapter_list" @tap="goChapter(item)">
                {{item+1}}
            </view>
        </view>		
	</view>
</template>

<script>
	export default {
        onLoad(options) {
            this.name = options.name
            this.total = options.total?Number(options.total):0
        },
		data() {
			return {
                name:'',
				total:0,
			}
		},
		methods: {
			goChapter(item){
                uni.showToast({
                    title:'进入' + item + '章!'
                })
            }
		}
	}
</script>

<style>
    .chapter_container{
        flex-wrap: wrap;
    }
    .chapter_title{
        text-align: center;
        font-size: 16px;
        font-weight: bold;
        padding: 10px 0;
    }
    .title_sub{
        color: #ccc;
        font-size: 12px;
        margin-left: 15px;
    }
    .chapter_list_container{
        display: flex;
        flex-wrap: wrap;
    }
    .chapter_list{
        min-width: 25px;
        padding: 5px;
        margin: 2px;
        background-color: #f6f6f6;
        text-align: center;        
    }
</style>
