<template>
  <div class="ball" :style="styleObject">{{number}}</div>
</template>
<script>
export default {
  name: "LottoBall",
  props: {
      number: Number,
  },
  computed: { // 기존값으로 계산하는 거니까. computed
    styleObject() {
    let background;
    let margin;
    if(this.number <= 10) {
        background = "red";
    } else if (this.number <= 20){
        background = "orange";
    } else if (this.number <= 30) {
        background = "yellow";
    } else if (this.number <= 40) {
        background = "green";
    } else {
        background = "blue"
    }
    return {
      background,
    };
    }
  },
};
</script>

<style>
.ball {
  display: inline-block;
  border-bottom: solid;
  border-radius: 20px;
  width: 40px;
  height: 40px;
  line-height: 40px;
  font-size: 20px;
  text-align: center;
  margin-right: 20px;
  color: black;
  
}
</style>