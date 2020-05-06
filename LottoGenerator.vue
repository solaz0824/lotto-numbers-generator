
<template>
  <div class="screen">
    <h1>Lucky Numbers</h1>
    <div id="result-screen">
      <lotto-ball v-for="ball in winBalls" :key="ball" :number="ball"></lotto-ball>
      <!-- ball in winBalls 에서 ball은 number v-bind:number 를 :number로 축약. 여기서 props로 전달해줘서 사용됨. -->
    </div>
    <div >
    <div v-if="bonus" id="bonus-text">Bonus</div>
    <lotto-ball v-if="bonus" :number="bonus"></lotto-ball>
    </div>
    <div v-if="redo">
    <button @click="onClickRedo" id="btn">Restart</button>
    </div>
  </div>
</template>

<script>
import LottoBall from "./LottoBall";
function getWinNumbers() {
  const candidate = Array(45)
    .fill()
    .map((v, i) => i + 1);
  const shuffle = [];
  while (candidate.length > 0) {
    shuffle.push(
      candidate.splice(Math.floor(Math.random() * candidate.length), 1)[0]
    );
  }
  const bonusNumber = shuffle[shuffle.length - 1];
  const winNumbers = shuffle.slice(0, 6).sort((p, c) => p - c);
  return [...winNumbers, bonusNumber];
}

const timeouts = [];
export default {
  components: {
    LottoBall //"lotto-ball": LottoBall, 인데 줄여서 사용가능. 파스칼케이스 -> 케밥케이스로 뷰가 자동변경해줌.
  },
  data() {
    return {
      winNumbers: getWinNumbers(), //미리 숫자 뽑아놓고
      winBalls: [], // 공이 차례로 표시되게. 시각적 용도로.
      bonus: null,
      redo: false
    };
  },
  methods: {
    onClickRedo() {
      this.winNumbers = getWinNumbers();
      this.winBalls = [];
      this.bonus = null;
      this.redo = false;
      this.showBalls();
    },
    showBalls() {
      for (let i = 0; i < this.winNumbers.length - 1; i++) {
        timeouts[i] = setTimeout(() => {
          this.winBalls.push(this.winNumbers[i]);
        }, (i + 1) * 1000); // 1초마다 하나씩 푸쉬.
      }
      timeouts[6] = setTimeout(() => {
        this.bonus = this.winNumbers[6];
        this.redo = true;
      }, 7000);
    }
  },
  mounted() {
    this.showBalls();
  },
  beforeDestroy() {
    timeouts.forEach(t => {
      clearTimeout(t);
    });
  }
  // watch:{
  //   bonus(value, oldValue){ // 현재값, 예전값
  //     if(value.length === 0) {
  //       this.showBalls();     // 버튼 누르면 winBalls가 빈배열이 되고 그러면 조건문 실행. watch 실행되서 this.showBalls(); 실행됨.
  //     }
  //   }
  // }
};
</script>

<style>
html {
   background-color: black;
   color: antiquewhite;
}
.screen {
    text-align: center;
    font-family: monospace;
    margin: 10%;
   
}
#result-screen {
  padding: 20px;
  margin-left: 20px;
}
#bonus-text {
  padding: 20px 20px 20px 0;
  color: aquamarine;
  font-size: 20px;
  font-weight: bold;
}
#btn {
  width: 100px;
  background-color: lavender;
  height: 50px;
  font-family: monospace;
  font-size: large;
  margin: 30px 15px 0 0;
}
</style>
