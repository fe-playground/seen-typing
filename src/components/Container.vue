<template>
	<div>
		<input type="text" v-model="userName">
		<button type="button" @click="startGame" :disabled="!userName.length">Start</button>
		<div class="container">
			<div>
				<template v-if="isStart">
					<span>{{this.gameHistory[this.gameHistory.findIndex(item => item.id === this.currentId)].time}}</span>
					<ul>
						<li v-for="(text, index) in textList" :key="`item-${index}`">
							<p>{{text}}</p>
							<p><input type="text" :readonly="activeIndex !== index" :ref="`text`" @keydown.enter="matchText(index, $event)">
							</p>
						</li>
					</ul>
					<span v-if="currentId">{{this.gameHistory[this.gameHistory.findIndex(item => item.id === this.currentId)].time}}초 걸렸음</span>
				</template>
			</div>
			<div>
				<ul class="rank__list">
					<li v-for="(history, index) in sortHistoryList" :key="index">{{index+1}}위, 유저명: {{history.name}}, 게임 타입 : {{history.type}}, 경과 시간 : {{history.time}}</li>
				</ul>
			</div>
		</div>
	</div>
</template>

<script>
    import { mapState, mapActions } from 'vuex'

	export default {
		name: "Container",
		props: {
			data: {
				type: Array,
				required: true
			}
		},
		data () {
			return {
				userName: '',
				gameType: null,
				textList: [],
				activeIndex: -1,
				isStart: false,
				isEnd: false,
				gameHistory: [],
				setTime: null,
				currentId: ''
			};
		},
		created () {
			this.newGame();
			this.gameHistory = JSON.parse(localStorage.getItem("history")) || [];
		},
		methods: {
			newGame () {
				this.gameType = Math.floor(Math.random() * this.data.length);
				this.textList = this.data[this.gameType];
			},
			startGame () {
				const uid = 'id-' + Math.random().toString(36).substr(2, 16);
				this.isStart = true;
			  this.currentId= uid;
			  this.gameHistory.push({id: uid, name: this.userName, type: this.gameType, time: 0});
				this.autoFocus(0);
				this.setTimer();
				this.setUserName(this.userName);
			},
			setTimer () {
				this.setTime = setInterval(() => {
					this.gameHistory.map((item) => {
						if(item.id=== this.currentId){
							item.time += 1;
						}
					});
				}, 1000)
			},
			matchText (index, e) {
				if (this.textList[index] === e.target.value) {
					index === this.textList.length - 1
						? this.endGame()
						: this.autoFocus(index + 1);
				} else {
					alert("텍스트가 일치하지 않습니다.");
				}
			},
			endGame () {
			clearInterval(this.setTime);
			this.userName='';
		  	this.currentId= '';
		  	this.isStart=false;
				localStorage.setItem("history", JSON.stringify(this.gameHistory));
			},
			autoFocus (index) {
				this.activeIndex = index;
				this.$nextTick(() => {
					const input = this.$refs.text[index];
					input.focus();
				});
			},
            ...mapActions([
                'setUserName'
            ]),
		},
		computed: {
			sortHistoryList: function () {
				const game = this.gameHistory;
				return game.sort((a, b) => a.time - b.time || a.type - b.type);
			}
		}
	};
</script>

<style scoped lang="scss">
	* {
		box-sizing: border-box;
	}
	input[type="text"]{
		height: 25px;
		vertical-align: middle;
	}
	button{
		vertical-align: middle;
		border-radius: 5px;
		font-size: 14px;
		cursor: pointer;
		line-height: 25px;
		padding: 0 20px;
		color: white;
		background-color: black;
		&:disabled{
			opacity: 0.8;
	 	}
	}
	.container{
		width: 800px;
		display: flex;
		align-content: space-between;
	}
	li {
		padding: 0;
		list-style: none;
		margin: 0;
	}
	.rank__list{
		margin-left: 30px;
		border: 1px solid #eee;
		padding: 20px;
		li{
			line-height: 25px;
		}
	}
</style>
