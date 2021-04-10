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
          <p>{{ newInput }}</p>
        </div>
      </div>
      <div class="cal-body">
        <template v-for="(btn, index) in btns">
          <div
            class="btn"
            :class="btn[1]"
            v-model="calInput"
            @click="inputVal(index, btn[0], btn[1])"
            :key="index"
          >
            <span>{{ btn[0] }}</span>
          </div>
        </template>
        <div class="btn ac" @click="acBtn"><span>AC</span></div>
        <div class="btn del" @click="delBtn"><span>⌫</span></div>
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
      calInput: [],
      newInput: "0",
      view: "",
      calAns: "0",
      preNum: "",
      symbolVal: ["+", "-", "×", "÷"],
      btns: [
        ["7", "num"],
        ["8", "num"],
        ["9", "num"],
        ["÷", "symbol"],
        ["4", "num"],
        ["5", "num"],
        ["6", "num"],
        ["×", "symbol"],
        ["1", "num"],
        ["2", "num"],
        ["3", "num"],
        ["+", "symbol"],
        ["0", "num"],
        ["00", "num"],
        [".", "dot"],
        ["-", "symbol"]
      ]
    };
  },
  methods: {
    inputVal(index, num, type) {
      console.log("first", this.calInput);
      // 判斷第一個值是symbol
      if (!this.calInput[0] && (type === "symbol" || type === "dot")) {
        this.calInput.push("0");
        this.calInput.push(num);
        this.newInput = this.calInput.join("");
        return;
      }

      let lastIndex = this.calInput.length - 1;
      let last = this.calInput[lastIndex];
      //判斷進來的值是symbol
      if (type === "symbol") {
        let valid = false;

        this.symbolVal.forEach(item => {
          if (last === item) {
            valid = true;
          }
        });

        if (valid) {
          //如果陣列最後一個值也是symbol就取代
          this.calInput.splice(lastIndex, 1, num);
        } else {
          this.calInput.push(num);
        }
      } else if (type === "dot") {
        //如果是 點
        let valid = false;
        let op = -1;
        this.symbolVal.forEach(item => {
          if (this.calInput.lastIndexOf(item) > op) {
            valid = true;
            op > this.calInput.indexOf(item)
              ? op
              : (op = this.calInput.indexOf(item));
            console.log(op);
          }
        });
        if (valid) {
          //有運算子
          if (this.calInput.indexOf(".", op) > 1) {
            //如果有點
            console.log(this.calInput.indexOf(".", op));
            return;
          } else {
            this.calInput.push(num);
          }
        } else {
          //沒有運算子
          if (this.calInput.indexOf(".") > 1 || last === ".") {
            return;
          } else {
            this.calInput.push(num);
          }
        }

        this.calInput.indexOf("x");
      } else if (num === "00" || num === "0") {
        if (!this.calInput[0]) {
          this.calInput.push("0");
          return;
        } else if (this.calInput[0] !== "0" || last !== "0") {
          if (num === "00") {
            this.calInput.push("0");
            this.calInput.push("0");
          } else {
            this.calInput.push("0");
          }
        } else {
          if (last === "." || this.calInput[2] === "0") {
            if (num === "00") {
              this.calInput.push("0");
              this.calInput.push("0");
            } else {
              this.calInput.push("0");
            }
          } else {
            return;
          }
        }
      } else if (this.calInput[0] === "0") {
        this.calInput.push(num);
      } else {
        this.calInput.push(num);
      }

      this.newInput = this.calInput.join("");
      console.log("first", this.calInput);

      // console.log("點擊Btn的陣列", this.calInput);
      // console.log("點擊Btn的值", this.newInput);
      // console.log("index0", this.calInput[0]);
      // console.log("index0", !this.calInput[0]);

      // console.log("index1", this.calInput[1]);
      // console.log("index1", !this.calInput[1]);
      // console.log(this.newInput);
    },
    delBtn() {
      if (!this.calInput[0] || !this.calInput[1]) {
        this.acBtn();
        console.log("安安");
      } else {
        this.calInput.pop();
        this.newInput = this.calInput.join("");
      }
      console.log("刪除後的陣列" + this.calInput);
      console.log("刪除後的值" + this.newInput);
    },
    acBtn() {
      this.calInput = [];
      this.newInput = "0";
      console.log("acBtn" + this.calInput);
      console.log("acBtn" + this.newInput);
    },
    getData(e) {
      // console.log(this.$refs.dataNum.dataset.num); // 输出 100
    }
  }
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
