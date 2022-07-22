<template>
	<scroll-view scroll-y="true" >
		<view class="content">
			<view class="placehold"></view>
			<view class="chat-item" v-for="chat in chats">
				<view class="item-wrap">
					<view class="msg-receive" v-if="!chat.mine">
						<img class="avatar" src="../../static/logo.png" alt="">
						<view class="msg-wrap">
							<text class="user">receive</text>
							<text class="msg">{{ chat.data }}</text>
						</view>
					</view>
				</view>
				<view class="item-wrap">
					<view class="msg-transmit" v-if="!!chat.mine">
						<img class="avatar" src="../../static/logo.png" alt="">
						<view class="msg-wrap">
							<text class="msg">{{ chat.data }}</text>
						</view>
					</view>
				</view>
			</view>
		</view>
	</scroll-view>
	<view class="console">
		<view class="voice">
			<uni-icons class="icon" type="mic"></uni-icons>
		</view>
		<view class="input">
			<input type="text" v-model="input" v-on:keyup.enter="send">
		</view>
		<view class="emo">
			<uni-icons class="icon" type="qq"></uni-icons>
		</view>
		<view class="action" v-on:click="send">
			<uni-icons class="icon" type="plus"></uni-icons>
		</view>
	</view>
</template>

<script setup>
	import {
		ref
	} from 'vue'
	const chats = ref([{
			mine: 0,
			type: 0,
			data: 'hello'
		},
		{
			mine: 1,
			type: 0,
			data: 'world'
		},
		{
			mine: 0,
			type: 0,
			data: 'hellwegehrtyujtykuyluioertgdsvfgeargo'
		},
		{
			mine: 1,
			type: 0,
			data: 'wgerherththrjttydfkthbfrrterhgerthyuityrkmg'
		},
	])
	
	const input = ref('')
	
	const socket = ref(false)
	
	uni.connectSocket({
		url: 'ws://127.0.0.1:9502',
	}).then(() => {
		return new Promise(resolve => {
			uni.onSocketOpen(() => {
				console.log('connected')
				resolve()
			})
		})
	}).then(() => {
		socket.value = true
	})
	
	function send() {
		console.log('send msg')
		if (input.value.length > 0 && socket) {
			uni.sendSocketMessage({
				data: input.value
			})
			
			input.value = ''
		}
	}
</script>

<style lang="scss" scoped>
	.content {
		display: flex;
		flex-direction: column-reverse;
		align-items: center;
		justify-content: center;
		.placehold {
			display: block;
			height: 100rpx;
		}
	}

	.chat-item {
		width: 100%;
		display: flex;
		justify-content: space-between;
		
		.item-wrap {
			width: 100%;
		}

		.msg-receive {
			display: flex;
			flex-direction: row;
			justify-content: center;
			align-items: center;
			width: 100%;
			margin: 18rpx 0;

			.msg-wrap {
				display: flex;
				flex-direction: column;
				width: 100%;

				.user {
					color: #6c6c6c;
					padding: 8rpx;
				}
			}
		}

		.msg-transmit {
			display: flex;
			flex-direction: row-reverse;
			justify-content: center;
			align-items: center;
			width: 100%;
			margin: 18rpx 0;

			.msg-wrap {
				display: flex;
				flex-direction: row-reverse;
				width: 100%;
			}
		}

		.avatar {
			margin: 0 16rpx;
			width: 66rpx;
			height: 66rpx;
		}

		.msg {
			display: block;
			background-color: #fbfbfb;
			padding: 8rpx 18rpx;
			border-radius: 8rpx;
			max-width: 400rpx;
			overflow-wrap: break-word;
			text-align: justify;
		}
	}
	.console {
		position: fixed;
		bottom: 0;
		display: flex;
		align-items: center;
		height: 100rpx;
		background-color: #e8e5ea;
		border-top: 1rpx solid #e6e4e9;
		width: 100vw;
		
		.voice {
			width: 60rpx;
			height: 60rpx;
			border-radius: 50%;
			margin: 0 20rpx;
		}
		
		.emo {
			width: 60rpx;
			height: 60rpx;
			border-radius: 50%;
			margin: 0 20rpx;
		}
		
		.action {
			width: 60rpx;
			height: 60rpx;
			border-radius: 50%;
		}
		.input {
			background-color: #f2f2f2;
			border-radius: 6rpx;
			height: 70rpx;
			width: 470rpx;
		}
		
		.icon {
			font-size: 60rpx!important;
			color: #4e4d56!important;
		}
	}
</style>
