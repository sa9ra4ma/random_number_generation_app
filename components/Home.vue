<template>
  <div>
    <!-- <div class="m-3">
      <button type="button" @click="displayNumber(0)" :class="{hiddenHome: !hiddenNameBtn[0]}" class="name-btn-home px-2 py-1 bg-blue-400 text-white font-semibold rounded hover:bg-blue-500">今福</button>
      <button type="button" @click="displayNumber(1)" :class="{hiddenHome: !hiddenNameBtn[1]}" class="name-btn-home px-2 py-1 bg-red-400 text-white font-semibold rounded hover:bg-red-500">今村</button>
      <button type="button" @click="displayNumber(2)" :class="{hiddenHome: !hiddenNameBtn[2]}" class="name-btn-home px-2 py-1 bg-green-400 text-white font-semibold rounded hover:bg-green-500">山内</button>
      <button type="button" @click="displayNumber(3)" :class="{hiddenHome: !hiddenNameBtn[3]}" class="name-btn-home px-2 py-1 bg-yellow-400 text-white font-semibold rounded hover:bg-yellow-500">湯浅</button>
      <button type="button" @click="displayNumber(4)" :class="{hiddenHome: !hiddenNameBtn[4]}" class="name-btn-home px-2 py-1 bg-pink-400 text-white font-semibold rounded hover:bg-pink-500">新福</button>
    </div>
    <div class="m-3" :class="{hiddenHome: randomNumber == 0}">
      <p>あなたの番号は{{ randomNumber }}です。</p>
    </div> -->
    <div class="m-3">
      <table class="table-auto">
        <thead>
          <tr>
            <th><button type="button" @click="displayNumber(0)" :class="{hiddenHome: hiddenNameBtn[0]}" class="name-btn-home px-2 py-1 bg-blue-400 text-white font-semibold rounded hover:bg-blue-500">今福</button></th>
            <th><button type="button" @click="displayNumber(1)" :class="{hiddenHome: hiddenNameBtn[1]}" class="name-btn-home px-2 py-1 bg-red-400 text-white font-semibold rounded hover:bg-red-500">今村</button></th>
            <th><button type="button" @click="displayNumber(2)" :class="{hiddenHome: hiddenNameBtn[2]}" class="name-btn-home px-2 py-1 bg-green-400 text-white font-semibold rounded hover:bg-green-500">山内</button></th>
            <th><button type="button" @click="displayNumber(3)" :class="{hiddenHome: hiddenNameBtn[3]}" class="name-btn-home px-2 py-1 bg-yellow-400 text-white font-semibold rounded hover:bg-yellow-500">湯浅</button></th>
            <th><button type="button" @click="displayNumber(4)" :class="{hiddenHome: hiddenNameBtn[4]}" class="name-btn-home px-2 py-1 bg-pink-400 text-white font-semibold rounded hover:bg-pink-500">新福</button></th>
          </tr>
        </thead>
        <tbody>
          <tr>
            <td :class="{hiddenHome: hiddenUserValue[0]}" class="text-center">{{ randomNumberList[0] }}</td>
            <td :class="{hiddenHome: hiddenUserValue[1]}" class="text-center">{{ randomNumberList[1] }}</td>
            <td :class="{hiddenHome: hiddenUserValue[2]}" class="text-center">{{ randomNumberList[2] }}</td>
            <td :class="{hiddenHome: hiddenUserValue[3]}" class="text-center">{{ randomNumberList[3] }}</td>
            <td :class="{hiddenHome: hiddenUserValue[4]}" class="text-center">{{ randomNumberList[4] }}</td>
          </tr>
        </tbody>
      </table>
    </div>
    <div class="m-3">
      <button type="button" @click="generatorRundomNumber" class="px-2 py-1 bg-gray-400 text-white font-semibold rounded hover:bg-bgray-500">乱数再生成</button>
    </div>
    <div class="m-3">
      <button type="button" @click="resultAnnouncement" class="px-2 py-1 bg-indigo-400 text-white font-semibold rounded hover:bg-indig-500">結果発表</button>
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
      randomNumberList: [0, 0, 0, 0, 0],
      hiddenNameBtn: [false, false, false, false, false],
      hiddenUserValue: [true, true, true, true, true],
      isFinish: false,
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
      this.hiddenUserValue[target] = false;
      if (!this.isFinish) {
        for (let i = 0; i < 5; i++) {
          if (i === target) continue;
          this.hiddenNameBtn[i] = true;
        }
      }

      // classのバインドが、配列の値では検知されないので、配列ごと入れ替える暫定対応
      this.hiddenNameBtn = JSON.parse(JSON.stringify(this.hiddenNameBtn));

      return;
    },
    generatorRundomNumber: async function () {
      const resConfirm = confirm('全員分の値が変更されますが、よろしいですか。');
      if (!resConfirm) return;
      try {
        const res = await axios.post('/api/randomnumber');
        if (res.status === 200) {
          this.randomNumberList = res.data;
          this.hiddenNameBtn = [false, false, false, false, false];
          this.hiddenUserValue = [true, true, true, true, true];
          this.isFinish = false;
        } else {
          alert('乱数の生成に失敗しました。');
        }
      } catch(err) {
        alert('乱数の生成に失敗しました。');
      }
      return;
    },
    resultAnnouncement: function () {
      this.isFinish = true;
      this.hiddenNameBtn = [false, false, false, false, false];
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
