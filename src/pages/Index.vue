<template>
  <q-page class=" flex" style="background-color: #243441;">
    <div style="position:absolute; top:10px ">
      <q-btn
        class="oka"
        label="Randomize"
        @click="generateBlocks()"
        color="primary"
      ></q-btn
      ><q-btn
        class="oka"
        label="bubbleSort"
        @click="bubbleSort()"
        color="primary"
      ></q-btn>
    </div>
    <section style="" class="data-container"></section>
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
    this.generateBlocks();
  },
  methods: {
    generateBlocks(num = 20) {
      for (var i = 0; i < num; i++) {
        this.randArr.push(i + 1);
      }
      this.randomize();
      const container = document.querySelector(".data-container");
      this.removeAllChildNodes(container);
      if (num && typeof num !== "number") {
        alert("First argument must be a typeof Number");
        return;
      }
      for (let i = 0; i < num; i += 1) {
        const value = this.randArr[i];
        const block = document.createElement("div");
        block.classList.add("block");
        block.style.height = `${value * 3}px`;
        block.style.margin = `0px`;
        block.style.width = `28Px`;
        block.style.float = `left`;
        block.style.background = "#58b7ff";

        block.style.transform = `translateX(${i * 30}px)`;
        block.style.transform = `translateY(-1)`;
        const blockLabel = document.createElement("label");
        blockLabel.classList.add("block__id");
        blockLabel.innerHTML = value;

        block.appendChild(blockLabel);
        container.appendChild(block);
      }
    },
    randomize() {
      this.randArr.sort(() => 0.5 - Math.random());
    },
    removeAllChildNodes(parent) {
      while (parent.firstChild) {
        parent.removeChild(parent.firstChild);
      }
    },
    swap(el1, el2) {
      const container = document.querySelector(".data-container");

      return new Promise(resolve => {
        const style1 = window.getComputedStyle(el1);
        const style2 = window.getComputedStyle(el2);

        const transform1 = style1.getPropertyValue("transform");
        const transform2 = style2.getPropertyValue("transform");

        el1.style.transform = transform2;
        el2.style.transform = transform1;

        // Wait for the transition to end!
        window.requestAnimationFrame(function() {
          setTimeout(() => {
            container.insertBefore(el2, el1);
            resolve();
          }, 250);
        });
      });
    },

    async bubbleSort(delay = 100) {
      if (delay && typeof delay !== "number") {
        alert("sort: First argument must be a typeof Number");
        return;
      }
      let blocks = document.getElementsByClassName("block");
      for (let i = 0; i < blocks.length - 1; i += 1) {
        for (let j = 0; j < blocks.length - i - 1; j += 1) {
          blocks[j].style.backgroundColor = "#FF4949";
          blocks[j + 1].style.backgroundColor = "#FF4949";

          await new Promise(resolve =>
            setTimeout(() => {
              resolve();
            }, delay)
          );

          const value1 = Number(blocks[j].childNodes[0].innerHTML);
          const value2 = Number(blocks[j + 1].childNodes[0].innerHTML);

          if (value1 > value2) {
            await this.swap(blocks[j], blocks[j + 1]);
            blocks = document.querySelectorAll(".block");
          }

          blocks[j].style.backgroundColor = "#58B7FF";
          blocks[j + 1].style.backgroundColor = "#58B7FF";
        }

        blocks[blocks.length - i - 1].style.backgroundColor = "#13CE66";
      }
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
* {
  font-family: sans-serif;
}

.header {
  margin-top: 20px;
  text-align: center;
  font-size: 20px;
}

.data-container {
  position: absolute;
  top: 50%;
  width: 600px;
  height: 384px;
}

.block {
  width: 28px;
  position: absolute;
  transition: 0.5s;
}

.block__id {
  position: absolute;
  top: -24px;
  width: 100%;
  text-align: center;
}
</style>
