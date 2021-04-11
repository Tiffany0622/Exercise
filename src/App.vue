<template>
  <div id="app">
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
        <div class="btn sum" @click="calculatBtn"><span>=</span></div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "App",
  data() {
    return {
      calInput: [],
      newInput: "0",
      opArr: [],
      numArr: [],
      calAns: "0",
      preNum: "",
      symbolVal: ["+", "-", "×", "÷"],
      sum: 0,
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
      // console.log("最初的陣列", this.calInput);
      let first = this.calInput[1];
      // console.log("first", first);
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
            // console.log(op);
          }
        });
        if (valid) {
          //有運算子
          if (this.calInput.indexOf(".", op) > 1) {
            //如果有點
            // console.log(this.calInput.indexOf(".", op));
            return;
          } else {
            this.calInput.push(num);
          }
        } else {
          //沒有運算子
          if (this.calInput.indexOf(".") >= 0 || last === ".") {
            return;
          } else {
            // console.log("不小心進來了啦");
            // console.log("look", this.calInput.indexOf("."));
            this.calInput.push(num);
          }
        }
        // this.calInput.indexOf("x");
      } else if (num === "00" || num === "0") {
        //判斷如果是0和00
        let valid = false;
        let op = -1;
        this.symbolVal.forEach(item => {
          if (this.calInput.lastIndexOf(item) > op) {
            valid = true;
            op > this.calInput.indexOf(item)
              ? op
              : (op = this.calInput.indexOf(item));
            // console.log(op);
          }
        });
        if (!this.calInput[0]) {
          //如果是空陣列
          this.calInput.push("0");
          this.newInput = this.calInput.join("");
          // console.log("我進來了空陣列");
        } else if (this.calInput[0] === "0" && !this.calInput[1]) {
          //如果陣列只有一個值且是0
          // console.log("第一位是0");
          return;
        } else if (valid) {
          //如果有運算子
          // console.log("進來囉運算子");
          if (this.calInput.indexOf(".", op) > 1 || this.calInput[op + 1] !== 0)
            //如果運算子之後有點 或 運算子之後不是０
            this.pushZero(num);
          // console.log("執行pushZero囉");
        } else if (!valid) {
          //如果沒有運算子
          if (this.calInput[0] !== "0") {
            // console.log("第一位不是0就推,執行pushZero囉");
            this.pushZero(num);
          }
          if (this.calInput[0] === "0" && this.calInput.indexOf(".") >= 0) {
            // console.log("第一位是0且有點就推");
            this.pushZero(num);
          }
        } else {
          // console.log("我也不知道為什麼會進來");
          this.pushZero(num);
        }
      } else if (this.calInput[0] === "0") {
        //第一位是0
        // console.log("嘿嘿第一位是0");
        this.calInput.splice(0, 1, num);
      } else {
        this.calInput.push(num);
      }
      this.newInput = this.calInput.join("");
      let newNum;
      // newNum = parseFloat(this.newInput);
      // console.log("this.newInput", this.newInput);
      // console.log("newInput型態是", typeof this.newInput);
      // console.log("newNum", newNum);
      // console.log("newNum型態是", typeof newNum);
      // console.log("點擊Btn的陣列", this.calInput);
      // console.log("點擊Btn的值", this.newInput);
    },
    pushZero(num) {
      if (num === "0") {
        this.calInput.push(num);
      }
      if (num === "00") {
        this.calInput.push("0");
        this.calInput.push("0");
      }
    },
    delBtn() {
      if (!this.calInput[0] || !this.calInput[1]) {
        this.acBtn();
        // console.log("安安");
      } else {
        this.calInput.pop();
        this.newInput = this.calInput.join("");
      }
      // console.log("刪除後的陣列" + this.calInput);
      // console.log("刪除後的值" + this.newInput);
    },
    acBtn() {
      this.calInput = [];
      this.newInput = "0";
      // console.log("acBtn" + this.calInput);
      // console.log("acBtn" + this.newInput);
    },
    getOp() {
      // 取得運算子
      this.opArr = this.calInput.filter(
        e => e === "+" || e === "-" || e === "÷" || e === "×"
      );
      //將 ×
      //將 × ÷ 轉換成 * /
      this.opArr = this.opArr.reduce(function(acc, cur) {
        if (cur === "×") {
          acc.push("*");
        } else if (cur === "÷") {
          acc.push("/");
        } else {
          acc.push(cur);
        }
        return acc;
      }, []);
    },
    getNum() {
      // 取得數值
      let str = this.calInput.reduce(function(acc, cur) {
        if (cur === "+" || cur === "-" || cur === "×" || cur === "÷") {
          cur = ",";
        }
        return acc + cur;
      }, "");
      // 字串轉陣列
      let arr = str.split(",");
      // 陣列裡字串轉數值
      this.numArr = arr.map(e => parseFloat(e));
    },
    toMulti() {
      let { opArr, numArr } = this;

      for (let i = 0; i < opArr.length; i) {
        let mul = opArr.indexOf("*");
        let di = opArr.indexOf("/");
        let idx = 0;
        if (mul < di && mul >= 0) {
          idx = mul;
        } else if (mul < di && mul < 0) {
          idx = di;
        } else if (mul > di && di >= 0) {
          idx = di;
        } else if (mul > di && di < 0) {
          idx = mul;
        } else {
          idx = -1;
        }

        if (idx < 0) return;

        if (opArr[idx] === "*") {
          numArr.splice(idx, 2, numArr[idx] * numArr[idx + 1]);
        } else {
          numArr.splice(idx, 2, numArr[idx] / numArr[idx + 1]);
        }
        opArr.splice(idx, 1);
        console.log(opArr.length, i);
      }
    },
    toPlus() {
      let { opArr, numArr } = this;

      for (let i = 0; i < opArr.length; i) {
        let plus = opArr.indexOf("+");
        let sub = opArr.indexOf("-");
        let idx = 0;
        if (plus < sub && plus >= 0) {
          idx = plus;
        } else if (plus < sub && plus < 0) {
          idx = sub;
        } else if (plus > sub && sub >= 0) {
          idx = sub;
        } else if (plus > sub && sub < 0) {
          idx = plus;
        } else {
          idx = -1;
        }

        if (idx < 0) return;

        if (opArr[idx] === "+") {
          numArr.splice(idx, 2, numArr[idx] + numArr[idx + 1]);
        } else {
          numArr.splice(idx, 2, numArr[idx] - numArr[idx + 1]);
        }
        opArr.splice(idx, 1);
      }
    },
    calculatBtn() {
      // console.log("終於來計算啦");
      const vm = this;
      const { calInput } = this;
      const c = calInput[calInput.length - 1];
      // 如果陣列最後一個值是運算子 return 不計算
      if (c === "+" || c === "-" || c === "÷" || c === "×") return;

      this.getOp();
      this.getNum();
      // let sum = [0];

      // 先乘除
      this.toMulti();

      // 後加減
      this.toPlus();

      this.calAns = this.numArr[0];
      console.log(this.opArr, this.numArr, this.opArr[0]);
    }
  }
};
</script>

<style src="./assets/css/all.css"></style>
