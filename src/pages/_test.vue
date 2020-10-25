<template>
  <div>
    <button @click="init">
      click me
    </button>
    <button @click="bubbleSort">
      sort
    </button>
    <div class="chart" style="height:100px"></div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      randarr: []
    };
  },
  methods: {
    init() {
      this.randarr = [];
      const container = document.querySelector(".chart");
      this.removeAllChildNodes(container);

      for (var i = 1; i <= 50; i++) {
        this.randarr.push(i);
      }
      this.randomize();

      for (var i = 0; i < 50; i++) {
        const block = document.createElement("div");
        const barLable = document.createElement("lable");
        block.classList.add("bar");
        block.style.width = "28px";
        block.style.backgroundColor = "blue";
        block.style.float = "left";
        block.style.backgroundColor = `rgb(70,70,${(this.randarr[i] / 10) *
          255}`;
        block.style.height = `${this.randarr[i] * 10}px`;
        barLable.innerHTML = this.randarr[i];
        block.appendChild(barLable);
        container.appendChild(block);
      }
      console.log(this.randarr);
      //this.bubbleSort()
    },
    removeAllChildNodes(parent) {
      while (parent.firstChild) {
        parent.removeChild(parent.firstChild);
      }
    },
    randomize() {
      this.randarr.sort(() => 0.5 - Math.random());
    },
    swap(el1, el2) {
      const container = document.querySelector(".chart");
      return new Promise(resolve => {
        const style1 = window.getComputedStyle(el1);
        const style2 = window.getComputedStyle(el2);

        const transform1 = style1.getPropertyValue("transform");
        const transform2 = style2.getPropertyValue("transform");

        el1.style.transform = transform2;
        el2.style.transform = transform1;

        console.log("swapping started!");
        // Wait for the transition to end!
        window.requestAnimationFrame(function() {
          setTimeout(() => {
            container.insertBefore(el2, el1);
            resolve();
          }, 2);
        });
      });
    },
    async bubbleSort() {
      var blocks = document.querySelectorAll(".bar");
      console.log("starting sort " + blocks.length);

      for (var i = 0; i < blocks.length - 1; i++) {
        for (var j = 0; j < blocks.length - i - 1; j++) {
          blocks[j].style.backgroundColor = "#FF4949";
          blocks[j + 1].style.backgroundColor = "#FF4949";

          await new Promise(resolve =>
            setTimeout(() => {
              resolve();
            }, 1)
          );

          let val1 = blocks[j].childNodes[0].innerHTML;
          let val2 = blocks[j + 1].childNodes[0].innerHTML;

          if (Number(val1) > Number(val2)) {
            await this.swap(blocks[j], blocks[j + 1]);
            blocks = document.querySelectorAll(".bar");
          }
          blocks[j].style.backgroundColor = "#58B7FF";
          blocks[j + 1].style.backgroundColor = "#58B7FF";
        }
        blocks[blocks.length - i - 1].style.backgroundColor = "#13CE66";
        console.log(blocks[i].childNodes[0].innerHTML + ' set to coorect')
      }
    }
  }
};
</script>

<style>
body {
  background: #20262e;
  padding: 20px;
  font-family: Helvetica;
}

#app {
  background: #fff;
  border-radius: 4px;
  padding: 20px;
}

.chart{
  display: flex;
  flex-direction:row-reverse
}
.bar {
  width: 28px;
  margin: 5px;
  background: "blue";
  float: left;
}
</style>
