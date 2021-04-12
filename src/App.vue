<template>
  <div id="app">
    <div class="calculator">
      <div class="cal-screen">
        <div class="cal-input">
          <p>{{ getNewInput }}</p>
        </div>
        <div class="cal-answer">
          <p :style="ansStyle">{{ calAns }}</p>
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
  computed: {
    ansStyle() {
      if (this.calAns.length <= 8) {
        return `fontSize:56px;`;
      }
      if (this.calAns.length > 9) {
        return `font-size:20px;  word-break:break-all;`;
      }
    },

    getNewInput() {
      if (!this.calInput[0] || this.numArr[0]) {
        //如果是空陣列
        //如果已送過等號 9+9=18 按點
        return this.newInput;
      }
      if (this.calInput.length <= 16) {
        //如果輸入長度小於等於16
        this.newInput = this.calInput.join("");
        this.calAns = this.newInput;
        return this.newInput;
      }
      if (this.calInput.length >= 17) {
        this.calInput.splice(16);
        this.newInput = this.calInput.join("");
        this.calAns = this.newInput;
        return this.newInput;
      }
    }
  },
  methods: {
    inputVal(index, num, type) {
      // 判斷第一個值是symbol
      if (!this.calInput[0] && (type === "symbol" || type === "dot")) {
        this.calInput.push("0");
        this.calInput.push(num);
        this.newInput = this.calInput.join("");
        return;
      }

      let lastIndex = this.calInput.length - 1;
      let last = this.calInput[lastIndex];
      let co = this.calInput;
      let o1 = co.indexOf("+");
      let o2 = co.indexOf("-");
      let o3 = co.indexOf("×");
      let o4 = co.indexOf("÷");
      //判斷進來的值是symbol
      if (type === "symbol") {
        if (this.calInput.length >= 17 && this.numArr[0]) {
          // 長度限制18 不能再輸入
          return;
        }
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
        // 找運算子索引位置
        this.symbolVal.forEach(item => {
          if (this.calInput.lastIndexOf(item) > op) {
            valid = true;
            op > this.calInput.indexOf(item)
              ? op
              : (op = this.calInput.indexOf(item));
          }
        });
        if (valid) {
          //有運算子
          if (this.calInput.indexOf(".", op) > 1) {
            //如果有點
            return;
          } else {
            this.calInput.push(num);
          }
        } else {
          //沒有運算子
          if (this.calInput.indexOf(".") >= 0 || last === ".") {
            return;
          } else {
            this.calInput.push(num);
          }
        }
      } else if (num === "00" || num === "0") {
        //判斷如果是0和00
        let valid = false;
        let op = -1;

        this.symbolVal.forEach(item => {
          if (this.calInput.lastIndexOf(item) > op) {
            // 從陣列最後開始找，如果有運算子
            valid = true;
            op > this.calInput.indexOf(item)
              ? op
              : (op = this.calInput.indexOf(item));
          }
        });
        if (!this.calInput[0]) {
          //如果是空陣列
          this.calInput.push("0");
          this.newInput = this.calInput.join("");
        } else if (this.calInput[0] === "0" && !this.calInput[1]) {
          //如果陣列只有一個值且是0
          return;
        } else if (valid) {
          //如果有運算子
          if (this.calInput[op + 1] && this.calInput.indexOf(".", op) < 0) {
            //第一位是零,沒有點
            return;
          } else {
            this.calInput.push("0");
            this.newInput = this.calInput.join("");
            return;
          }
        } else if (!valid) {
          this.pushZero(num);
        }
      } else if (
        // this.numArr[0] && (last === "+" || last === "-" || last === "÷" || last === "×")) {
        this.numArr[0] &&
        (o1 > -1 || o2 > -1 || o3 > -1 || o4 > -1)
      ) {
        this.calInput.push(num);
        this.newInput = this.calInput.join("");
        this.calAns = this.newInput;
        console.log("BBB");
      } else if (this.calInput === "0") {
        // } else if (this.calInput[0] === "0") {
        //第一位是0 已送過等號又直接按數字
        console.log("TTTTTT");
        this.acBtn();
        this.calInput.push(num);
      } else {
        this.calInput.push(num);
      }
      this.newInput = this.calInput.join("");
      this.calAns = this.newInput;
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
    //⌫
    delBtn() {
      if (this.numArr[0]) {
        //判斷已送出等號
        this.numToArr();
        this.newInput = "";
        return;
      }
      if ((this.calAns = "0") && !this.calInput[0]) {
        return;
      }
      if (!this.numArr[0]) {
        this.calInput.pop();
        this.newInput = this.calInput.join("");
      }
    },
    // AC
    acBtn() {
      this.calInput.length = 0;
      this.numArr.length = 0;
      this.newInput = "0";
      this.calAns = "0";
    },
    getOp() {
      // 取得運算子
      this.opArr = this.calInput.filter(
        e => e === "+" || e === "-" || e === "÷" || e === "×"
      );
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
    numToArr() {
      let ansNum = this.numArr[0]; //type number
      let ansStr = ansNum.toString(); // type String
      this.calAns = ansStr;
      let ansArr = ansStr.split(""); //type Array
      this.calInput = ansArr;
    },
    calculatBtn() {
      const vm = this;
      const { calInput } = this;
      const c = calInput[calInput.length - 1];
      const n = calInput[calInput.length - 2];
      // 如果陣列最後一個值是運算子推進去運算子前面的值
      if (c === "+" || c === "-" || c === "÷" || c === "×") {
        calInput.push(n);
        this.newInput = calInput.join("");
        this.calAns = this.newInput;
      }

      //送等號符號
      let eqlArr = this.newInput.split("");
      let equIdx = eqlArr.indexOf("=");
      if (equIdx > -1) {
        eqlArr.splice(equIdx, 1);
        this.newInput = eqlArr.join("");
      }
      this.newInput = this.newInput.concat("=");

      //如果0的時候按等號
      if (this.calAns === "0") return;

      //如果數字長度達17又按等號
      let co = this.calInput;
      let o1 = co.indexOf("+");
      if (this.calInput.length === 17 && o1 === -1) {
        this.newInput = this.calInput.join("");
        this.calAns = this.newInput;
        return;
      }

      this.getOp();
      this.getNum();
      // let sum = [0];

      // 先乘除
      this.toMulti();

      // 後加減
      this.toPlus();

      // 將答案轉乘陣列推進去
      this.numToArr();
    }
  }
};
</script>

<style src="./assets/css/all.css"></style>
