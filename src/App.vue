<template>
  <h1>JIS G 4310対応計算機 ver 0.1</h1>
  <div class="f-container">
    <div class="f-item">
      比重：<input v-model="spgr" type="number" />(kg/mm*m<sup>2</sup>)<br/>
      板厚：<input v-model="thickness" type="number" />(mm)
      <hr/>
      単位質量<br/>
      計算値：{{ calcUnitMass() }}(kg/m<sup>2</sup>)<br/>
      有効値：{{ calcPrecision(calcUnitMass(), 4) }}(kg/m<sup>2</sup>)
    </div>
    <div class="f-item">
      幅：<input v-model="width" type="number" />(mm)<br/>
      長さ：<input v-model="length" type="number" />(mm)
      <hr/>
      面積<br/>
      計算値：{{ calcArea() }}(m<sup>2</sup>)<br/>
      有効値：{{ calcPrecision(calcArea(), 4) }}(m<sup>2</sup>)
    </div>
    <div class="f-item">
      算出質量(kg)<br/>
      計算値：{{ calcMass() }}(kg)<br/>
      有効値：{{ calcPrecision(calcMass(), 3) }}(kg)
    </div>
  </div>
  <hr/>
  メモ：丸めについてはHALF_EVEN（銀行丸め／JIS丸め）を使用しています
</template>

<script>
export default {
  name: 'App',
  data() {
    return {
      spgr: 7.93,
      thickness: 5,
      width: 1524,
      length: 3048
    }
  },
  methods: {
    calcUnitMass() {
      const BigNumber = require('bignumber.js');
      BigNumber.config({ ROUNDING_MODE: BigNumber.ROUND_HALF_EVEN });
      return BigNumber(this.spgr).times(this.thickness).toNumber();
    },
    calcArea() {
      const BigNumber = require('bignumber.js');
      BigNumber.config({ ROUNDING_MODE: BigNumber.ROUND_HALF_EVEN });
      return BigNumber(this.width).times(this.length).div(1000000).toNumber();
    },
    calcMass() {
      const BigNumber = require('bignumber.js');
      BigNumber.config({ ROUNDING_MODE: BigNumber.ROUND_HALF_EVEN });
      let unitMass = BigNumber(this.spgr).times(this.thickness).toPrecision(4);
      let area = BigNumber(this.width).times(this.length).div(1000000).toPrecision(4);
      return BigNumber(unitMass).times(area).toNumber();
    },
    // num:number(数字)
    // sd:significant digits(有効桁数)
    calcPrecision(num, sd){
      const BigNumber = require('bignumber.js');
      BigNumber.config({ ROUNDING_MODE: BigNumber.ROUND_HALF_EVEN });
      let result = BigNumber(num).toPrecision(sd);
      return BigNumber(result).toNumber();
    }
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  margin-top: 60px;
}
.f-container {
    display: flex;
}
.f-item {
    margin:20px
}
</style>
