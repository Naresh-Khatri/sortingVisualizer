<template>
  <q-page class="flex-center flex" style="background-color: #243441;">
    <div style="position:absolute; top:10px ">
      <q-btn label="Randomize" @click="randomize()" color="primary"></q-btn
      ><q-btn label="bubbleSort" @click="bubbleSort()" color="primary"></q-btn>
    </div>
    <div>
      <q-card style="height:500px; width:1030px;">
        <div
          v-for="(height, index) in randArr"
          :key="index + update"
          :style="{
            height: `${height * 10}px`,
            background: `black`,
            opacity: `${100 - (height * 100) / 50 + 10}%`
          }"
          style="border-radius:0px;"
          class="bar"
        ></div>
      </q-card>
    </div>
  </q-page>
</template>

<script>
export default {
  name: "PageIndex",
  data() {
    return {
      randArr: [],
      update: 0,
      currentIndex: 0
    };
  },
  mounted() {
    this.createarr();
  },
  methods: {
    createarr() {
      for (var i = 1; i < 50; i++) {
        this.randArr.push(i);
      }
      this.randomize();
    },
    randomize() {
      this.randArr.sort(() => 0.5 - Math.random());
    },
    async bubbleSort() {
      var i, j;
      let bars = document.getElementsByClassName('bar')
      for (i = 0; i < this.randArr.length; i++) {
        for (j = 0; j = this.randArr.length - i - 1; j++) {
          if (this.randArr[j] > this.randArr[j + 1]) {
            if (this.randArr[j] > this.randArr[j + 1]) {
              bars[this.randArr[j]].style.background = 'blue'
              bars[this.randArr[j+1]].style.background = 'blue'
              this.swap(j);
            }
          }
        }
      }
    },
    async swap(j) {
      return new Promise(resolve => {
        console.log("swapped!" + this.randArr[j]);
        window.requestAnimationFrame(function() {
          setTimeout(() => {
            var temp;
            temp = this.randArr[j];
            this.randArr[j] = this.randArr[j + 1];
            this.randArr[j + 1] = temp;
            this.update++;

            resolve();
          }, 250);
        });
      });
    }
  }
};
</script>
<style scoped>
.bar {
  width: 20px;
  margin-right: 1px;
  border: 1px solid black;
  float: left;
  transition: 0.5s;
  border-radius: 5px;
}
</style>
