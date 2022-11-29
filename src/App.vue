<script>
import frise from "./assets/frise.png"
import fleche from "./assets/fleche.png"

export default {
  name: "App",
  data() {
    return {
      colors: ["blue", "red", "green"],
      frise: frise,
      fleche: fleche,
      lines: [
        {
          id: 0,
          upper: null,
          down: null,
          result: null
        }
      ],
      selNum: null,
      selMul: null,
      multiplier: null,
      retenueGauche: []
    }
  },
  methods: {
    fretenueGauche: function (number) {
      console.log(number)
      this.retenueGauche[number] = this.selNum
    },
    selectN: function (number) {
      this.selNum = number
    },
    selectM: function (op) {
      this.selMul = op
    },
    mClick: function () {
      this.multiplier = this.selMul
      this.selMul = null
    },
    cClick: function (caseN, lineID) {
      if (this.lines[lineID][caseN] != null) {
        if (this.selNum != null) {
          this.lines[lineID][caseN] = this.selNum
        } else {
          this.lines[lineID][caseN] = null
        }
        this.checkLine()
        this.selNum = null;
        return;
      }
      this.lines[lineID][caseN] = this.selNum;
      this.selNum = null;
      this.checkLine()
    },
    checkLine() {
      let last = this.lines.at(-1)
      if (last["upper"] || last["down"] || last["result"]) {
        this.lines.push({ id: last["id"] + 1, upper: null, down: null, result: null })
      } else {
        let last1 = this.lines[last["id"] - 1]
        if (!(last1["upper"] || last1["down"] || last["result"])) {
          this.lines.pop()
        }
      }
    }
  }
}
</script>

<template>
  <div class="left">
    <div class="numbers">
      <div class="number" v-for="number in [...Array(10).keys()].map(String)" :key="number" @click="selectN(number)">
        <p>{{ number }}</p>
      </div>
    </div>
  </div>
  <div class="multipliers">
    <div v-for="operator in ['+', '-', 'X']" :key="operator" @click="selectM(operator)">
      {{ operator }}
    </div>
  </div>
  <div class="table">
    <img class="fleche" :src="fleche" height="60" />
    <div class="divider">
      <div class="lines">
        <div class="line" v-for="line in lines" :key="line">
          <div style="grid-row: 1;" class="retenue"
            :style="{ backgroundColor: colors[line['id'] - 3 * Math.floor(line['id'] / 3)] }"
            @click="cClick('retenue', line['id'])">
            {{ line["retenue"] }}
          </div>
          <div :style="{ backgroundColor: colors[line['id'] - 3 * Math.floor(line['id'] / 3)] }"
            @click="cClick('upper', line['id'])" class="case">
            {{ line["upper"] }}
          </div>
          <div :style="{ backgroundColor: colors[line['id'] - 3 * Math.floor(line['id'] / 3)] }"
            @click="cClick('down', line['id'])" class="case">
            {{ line["down"] }}
          </div>
          <div :style="{ backgroundColor: colors[line['id'] - 3 * Math.floor(line['id'] / 3)] }"
            @click="cClick('result', line['id'])" class="case">
            {{ line["result"] }}
          </div>
        </div>
      </div>
      <div class="operatorPos" >
        <div class="retenue" @click="fretenueGauche(0)">{{ retenueGauche[0] }}</div>
        <div @click="mClick" class="case operator">{{
            multiplier
        }}</div>
        <div class="retenue" style="grid-row: 4" @click="fretenueGauche(1)">{{ retenueGauche[1] }}</div>
      </div>
    </div>
    <div class="dLine"></div>
  </div>
  <div class="compteur"></div>
  <img class="frise" :src="frise" height="240" />
</template>

<style lang="scss">
@import url('https://fonts.googleapis.com/css2?family=Roboto:wght@500&display=swap');

* {
  -webkit-touch-callout: none;
  -webkit-user-select: none;
  -khtml-user-select: none;
  -moz-user-select: none;
  -ms-user-select: none;
  user-select: none;
}

.fleche {
  position: relative;
  left: 50%;
  transform: translate(-50%, 0%);
}

.frise {
  position: absolute;
  left: 50%;
  bottom: 0px;
  z-index: -1;
  transform: translate(-50%, 0);
}

* {
  font-family: 'Roboto', sans-serif;
}

.divider {
  display: flex;
  flex-direction: row-reverse;
}

.operator {
  grid-row: 3;
}

.operatorPos {
  grid-template-columns: 54px 15px;
  display: grid;
  grid-auto-rows: 54px;
  gap: 15px 0;
  margin: 5px;
}

.case {
  display: flex;
  align-items: center;
  justify-content: center;
  color: white;
  font-size: 36px;
  background-color: #ddcecd;
  border-radius: 10px;
  border: 2px solid rgba(0, 0, 0, .2);
}

.retenue {
  display: flex;
  align-items: center;
  justify-content: center;
  color: white;
  font-size: 36px;
  background-color: #ddcecd;
  border-radius: 50%;
  border: 2px solid rgba(0, 0, 0, .2);
}

.table {
  position: fixed;
  left: 50%;
  top: 50%;
  transform: translate(-50%, -50%);

  .dLine {
    height: 2px;
    background-color: black;
    margin-top: -67px;
    z-index: -1;
  }

  .line {
    display: grid;
    grid-auto-rows: 54px;
    grid-auto-columns: 54px;
    gap: 15px 0;
    margin: 5px;
  }
}

.lines {
  display: flex;
  flex-direction: row;
}

.multipliers {
  cursor: pointer;
  display: grid;
  grid-auto-flow: column;
  grid-auto-columns: 52px;
  grid-auto-rows: 52px;
  gap: 0 15px;
  top: 5px;
  position: fixed;


  div {
    background-color: #19647e;
    border-radius: 10px;
    color: white;
    font-size: 36px;
    display: flex;
    align-items: center;
    justify-content: center;
    transition: opacity 0.2s;

    &:hover {
      opacity: 70%;
    }

    p {
      margin: 0px;
    }
  }
}

.left {
  position: fixed;
  right: 15px;
  display: flex;
  align-items: center;
  top: 0px;
  height: 100%;
}

.numbers {
  cursor: pointer;
  display: grid;
  grid-auto-rows: 52px;
  grid-auto-columns: 52px;
  gap: 15px 0;


  .number {
    display: flex;
    background-color: #28afb0;
    border-radius: 10px;
    align-items: center;
    justify-content: center;
    transition: opacity 0.2s;

    &:hover {
      opacity: 70%;
    }
  }

  p {
    margin: 0px;
    color: white;
    font-size: 36px;
  }
}

@media (max-height: 666px) {
  .numbers {
    gap: 5px 5px;
  }
}

@media(max-height: 816px) {
  img {
    top: 0px;
  }

  .multipliers {
    top: 100%;
    transform: translate(0, -115%);
  }
}

//666 px
</style>