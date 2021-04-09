<template>
  <div id="app">
    <div class="test">
      <button data-num="" ref="dataNum" @click="getData($event)">点我</button>
    </div>
    <!-- <img src="./assets/logo.png" /> -->
    <!-- <HelloWorld /> -->
    <div class="calculator">
      <div class="cal-screen">
        <div class="cal-input">
          <p>{{ newInput }}</p>
        </div>
        <div class="cal-answer">
          <p>{{ calAns }}</p>
        </div>
      </div>
      <div class="cal-body">
        <div class="btn num" v-model="calInput" @click="inputVal(7)">
          <span>7</span>
        </div>
        <div class="btn num" v-model="calInput" @click="inputVal(8)">
          <span>8</span>
        </div>
        <div class="btn num" v-model="calInput" @click="inputVal(9)">
          <span>9</span>
        </div>
        <div class="btn symbol" v-model="calInput" @click="inputVal('÷')">
          <!-- <div class="btn symbol" v-model="calInput" @click="inputVal('÷')"> -->
          <span>÷</span>
        </div>
        <div class="btn num" v-model="calInput" @click="inputVal(4)">
          <span>4</span>
        </div>
        <div class="btn num" v-model="calInput" @click="inputVal(5)">
          <span>5</span>
        </div>
        <div class="btn num" v-model="calInput" @click="inputVal(6)">
          <span>6</span>
        </div>
        <div
          class="btn symbol"
          v-model="calInput"
          data-math="×"
          @click="inputVal('×')"
        >
          <span>×</span>
        </div>
        <div class="btn num" v-model="calInput" @click="inputVal(1)">
          <span>1</span>
        </div>
        <div class="btn num" v-model="calInput" @click="inputVal(2)">
          <span>2</span>
        </div>
        <div class="btn num" v-model="calInput" @click="inputVal(3)">
          <span>3</span>
        </div>
        <div class="btn symbol" v-model="calInput" @click="inputVal('+')">
          <span>+</span>
        </div>
        <div class="btn num" v-model="calInput" @click="inputVal(0)">
          <span>0</span>
        </div>
        <div class="btn num" v-model="calInput" @click="inputVal(0)">
          <span>00</span>
        </div>
        <div class="btn num" v-model="calInput" @click="inputVal('.')">
          <span>.</span>
        </div>
        <div class="btn symbol" v-model="calInput" @click="inputVal('-')">
          <span>-</span>
        </div>
        <div class="btn ac"><span>AC</span></div>
        <div class="btn del"><span>⌫</span></div>
        <div class="btn sum"><span>=</span></div>
      </div>
    </div>
  </div>
</template>

<script>
import HelloWorld from "./components/HelloWorld";

// 點擊按鈕 fn()
// 1.視窗顯示 用變數[]接 calInput = "字串相加" 
//   1.1判斷第1個值
//      1.1.1 是運算子 ==> 0 + "運算子"
//
//        
// 2.運算
// 判斷點擊運算子
// 111+222+444 = 
// 333 = 111+222
// 777 = 333+444
// preNum = preNum + newNum 


export default {
  name: "App",
  components: {
    HelloWorld
  },
  data() {
    return {
      calInput:[],
      newInput:'',
      view:'',
      calAns: "0",
      preNum:'',
      
    };
  },
  methods: {
    inputVal(num) {
      // this.calInput = this.calInput === "0" ? `${num}` : `${this.calInput}${num}`;
      this.calInput.push(num);
      if (this.calInput[0] === 0|| this.calInput[0] === "+") return this.calInput.shift();
      this.newInput = this.calInput.join('');
      console.log('index 0 是'+this.calInput[0]);
      console.log(this.calInput);
      console.log(this.newInput);
      
      // ||"+"||"-"||"×"||"÷" 
      // this.calAns = this.calInput;
    },

    getData(e) {
      console.log(this.$refs.dataNum.dataset.num); // 输出 100
    }
  },
};
</script>

<style src="./assets/css/all.css">
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
