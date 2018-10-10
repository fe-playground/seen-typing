<template>
    <div>
        <button type="button" @click="startGame">Start</button>
        <ul>
            <li v-for="(text, index) in textList" :key="`item-${index}`">
                <p>{{text}}</p>
                <p><input type="text" :readonly="activeIndex !== index" :ref="`text`" @keydown.enter="matchText(index, $event)"></p>
            </li>
        </ul>
        <button type="button" @click="endGame" :disabled="!isEnd">Close</button>
        <span v-if="timeDiffer">{{timeDiffer}}초 경과</span>
        <ul>
            <li v-for="history in sortHistoryList">게임 타입 : {{history.type}}, 경과 시간 : {{history.time}}</li>
        </ul>
    </div>
</template>

<script>
export default {
  name: "Container",
  props: {
    data: {
      type: Array,
      required: true
    }
  },
  data() {
    return {
      gameType: null,
      textList: [],
      start: null,
      end: null,
      timeDiffer: null,
      activeIndex: -1,
      isEnd: false,
      gameHistory: []
    };
  },
  created() {
    this.newGame();
    this.gameHistory = JSON.parse(localStorage.getItem("history")) || [];
  },
  methods: {
    newGame() {
      this.gameType = Math.floor(Math.random() * this.data.length);
      this.textList = this.data[this.gameType];
    },
    startGame() {
      this.start = new Date().getTime();
      this.autoFocus(0);
    },
    matchText(index, e) {
      if (this.textList[index] === e.target.value) {
        index === this.textList.length - 1
          ? (this.isEnd = true)
          : this.autoFocus(index + 1);
      } else {
        alert("텍스트가 일치하지 않습니다.");
      }
    },
    endGame() {
      this.end = new Date().getTime();
      this.timeDiffer = (this.end - this.start) / 1000;
      this.gameHistory.push({ type: this.gameType, time: this.timeDiffer });
      localStorage.setItem("history", JSON.stringify(this.gameHistory));
    },
    autoFocus(index) {
      this.activeIndex = index;
      this.$nextTick(() => {
        const input = this.$refs.text[index];
        input.focus();
      });
    }
  },
  computed: {
    sortHistoryList: function() {
      return this.gameHistory.sort((a, b) => a.time - b.time);
    }
  }
};
</script>

<style scoped>
</style>
