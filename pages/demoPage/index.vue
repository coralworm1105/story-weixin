<template>
	<view class="content">
		<view class="disc">
			<image :src="discSrc"></image>
		</view>
		<view class="storyTitle">
			{{audio[now].name}}
		</view>
		<view class="subTitle">
			{{subTitle}}
		</view>
		<uni-icon class="list-icon" color="#169af3" type="list" size="30" @click="openList"></uni-icon>
		<imt-audio continue :src="audio[now].src" :duration="audio[now].duration" :autoplay="true" @prev="now = now === 0?audio.length-1:now-1"
		 @next="now = now === audio.length-1?0:now+1"></imt-audio>
		<!-- 
			 src: String 音频链接*必须*
			 duration: Number 总时长（单位：s）*必须*
			 autoplay: Boolean 是否自动播放*默认false*
			 control: Boolean 是否需要上一曲/下一曲按钮*默认true*
			 continue:Boolean 播放完成后是否继续播放下一首，需定义@next事件*默认false*
			 color: String 主色调*默认#169af3*
			 @prev:点击上一首按钮
			 @next:点击下一首按钮
		  -->
		<uni-popup ref="popup" type="bottom">
			<view class="top">
				<view class="list-title">
					故事列表
				</view>
				<uni-icon type="close" class="list-close" color="#169af3" size="30"></uni-icon>
			</view>
			<view class="list" :class="{active:key===now}" v-for="(item,key) in audio" :key="key" @click="now = key">
				
				<view class="list-title">{{item.name}}</view>
				<image class="list-pic" src="http://mouyizhan.com/music.gif"></image>
			</view>			
		</uni-popup>

	</view>
</template>

<script>
	import imtAudio from '../../components/imt-audio/imt-audio';
	import uniIcon from '../../components/uni-icon/uni-icon.vue';
	import uniPopup from '@/components/uni-popup/uni-popup.vue';
	export default {
		data() {
			return {
				audio: [{
						src: 'https://resource.alilo.com.cn/res/5,0153ebd7abefe3.mp3',
						name: '01五颗松树小屋①',
						duration: 120
					},
					{
						src: 'https://resource.alilo.com.cn/res/7,01367105a7dc7c.mp3',
						name: '02五颗松树小屋②',
						duration: 237
					},
					{
						src: 'https://resource.alilo.com.cn/res/3,015a7216371e78.mp3',
						name: '03五颗松树小屋③',
						duration: 156
					},
					{
						src: 'https://resource.alilo.com.cn/res/4,015a5bc5be2d23.mp3',
						name: '04五颗松树小屋④',
						duration: 434
					}
				],
				now: 0,
				subTitle: "聪明的狐狸-布谷布谷讲故事",
				discSrc: "//qna.smzdm.com/201802/27/5a953762d1c463378.jpg_a200.jpg"
			}
		},
		components: {
			imtAudio,
			uniIcon,
			uniPopup
		},
		methods:{
			openList(){
				this.$refs.popup.open();
			}
		}
	}
</script>

<style lang="scss">
	page{
		background: #f5f5f5;
	}
	.content {
		padding: 20upx;
		position:relative;
		.disc{
			width:12rem;
			height:12rem;
			margin:2rem auto;
			border-radius: 12rem;
			border:3px solid #007AFF;
			overflow:hidden;
			animation: rotateWise 10s 0s infinite;
		}
		.storyTitle{
			text-align: center;
			font-size:1rem;
			line-height:2rem;
		}
		.subTitle{
			text-align: center;
			font-size:0.8rem;
			line-height:2rem;
			margin-bottom:1rem;
		}
		.list-icon{
			position:absolute;
			right:10px;
			top:18px;
			width:30px;
			height:30px;
		}
		.top{
			position:relative;
			.list-title{
				text-align:center;
			}
			.list-close{
				position:absolute;
				right:10px;
				top:-2px;
			}
		}
		.list {
			display: flex;
			justify-content: space-between;
			align-items: center;
			padding: 0 30upx;
			background: #fff;
			border-radius: 10upx;
			margin-top: 20upx;
			color: #333;

			.list-title {
				font-size: 28upx;
				line-height: 88upx;
			}

			.list-pic {
				display: none;
				width: 28upx;
				height: 28upx;
			}

			.active {
				background: #169af3;
				color: #fff;
			}

			.active .list-pic {
				display: block;
			}
		}
	}
	  @keyframes rotateWise {
		50% {
			transform: rotate(180deg);
		}
		100% {
			transform: rotate(360deg);
		}
	  }
</style>