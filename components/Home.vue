<template>
  <div>
    <div class="m-3">
      <button type="button" @click="displayNumber(0)" :class="{hiddenHome: !nameBtnStatus[0]}" class="name-btn-home px-2 py-1 bg-blue-400 text-white font-semibold rounded hover:bg-blue-500">今福</button>
      <button type="button" @click="displayNumber(1)" :class="{hiddenHome: !nameBtnStatus[1]}" class="name-btn-home px-2 py-1 bg-red-400 text-white font-semibold rounded hover:bg-red-500">今村</button>
      <button type="button" @click="displayNumber(2)" :class="{hiddenHome: !nameBtnStatus[2]}" class="name-btn-home px-2 py-1 bg-green-400 text-white font-semibold rounded hover:bg-green-500">山内</button>
      <button type="button" @click="displayNumber(3)" :class="{hiddenHome: !nameBtnStatus[3]}" class="name-btn-home px-2 py-1 bg-yellow-400 text-white font-semibold rounded hover:bg-yellow-500">湯浅</button>
      <button type="button" @click="displayNumber(4)" :class="{hiddenHome: !nameBtnStatus[4]}" class="name-btn-home px-2 py-1 bg-pink-400 text-white font-semibold rounded hover:bg-pink-500">新福</button>
    </div>
    <div class="m-3" :class="{hiddenHome: randomNumber == 0}">
      <p>あなたの番号は{{ randomNumber }}です。</p>
    </div>
    <div class="m-3">
      <button type="button" @click="generatorRundomNumber" class="px-2 py-1 bg-gray-400 text-white font-semibold rounded hover:bg-bgray-500">乱数再生成</button>
    </div>
  </div>
</template>

<script>
import axios from 'axios';
export default {
  name: 'Home',
  data: function() {
    return {
      randomNumber: 0,
      randomNumberList: [],
      nameBtnStatus: [true, true, true, true, true]
    }
  },
  computed: {
    // randomNumberStr: function () {
    //   return this.randomNumber ? `あなたの番号は${this.randomNumber}です。` : '';
    // }
  },
  mounted: async function () {
    try {
      const res = await axios.get('/api/randomnumber');
      if (res.status === 200) {
        this.randomNumberList = res.data;
      } else {
        alert('乱数の取得に失敗しました。');
      }
    } catch(err) {
      alert('乱数の取得に失敗しました。');
    }
    return;
  },
  methods: {
    displayNumber: function (target) {
      this.randomNumber = this.randomNumberList[target];
      for (let i = 0; i < 5; i++) {
        if (i === target) continue;
        this.nameBtnStatus[i] = false;
      }
      return;
    },
    generatorRundomNumber: async function () {
      try {
        const res = await axios.post('/api/randomnumber');
        if (res.status === 200) {
          this.randomNumberList = res.data;
          alert('乱数の生成に成功しました。');
          this.nameBtnStatus = [true, true, true, true, true];
          this.randomNumber = 0;
        } else {
          alert('乱数の生成に失敗しました。');
        }
      } catch(err) {
        alert('乱数の生成に失敗しました。');
      }
      return;
    }
  }
}
</script>

<style scoped>
.hiddenHome {
  visibility: hidden;
}
.name-btn-home {
  margin: 0 10px;
}
</style>
