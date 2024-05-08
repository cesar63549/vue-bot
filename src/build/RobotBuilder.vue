<template>
    <div class="content">
      <button class="add-to-cart" @click="addToCart()">
        Add to cart
      </button>
        <div class="top-row">
          <div class="top part">
            <div class="robot-name">
              {{selectedRobot.head.title}}
              <span v-if="selectedRobot.head.onSale" class="sale">
                Sale!
              </span>
            </div>
            <img :src="selectedRobot.head.src" title="head"/>
            <button @click="selectPreviousHead()" class="prev-selector">&#9668;</button>
            <button v-on:click="selectNextHead()" class="next-selector">&#9658;</button>
          </div>
        </div>
        <div class="middle-row">
          <div class="left part">
            <img v-bind:src="selectedRobot.leftArm.src" title="left arm"/>
            <button v-on:click="selectPreviousLeftArm()" class="prev-selector">&#9650;</button>
            <button v-on:click="selectNextLeftArm()" class="next-selector">&#9660;</button>
          </div>
          <div class="center part">
            <img v-bind:src="selectedRobot.center.src" title="center"/>
            <button v-on:click="selectPreviousCenter()" class="prev-selector">&#9668;</button>
            <button v-on:click="selectNextCenter()" class="next-selector">&#9658;</button>
          </div>
          <div class="right part">
            <img v-bind:src="selectedRobot.rightArm.src" title="right arm"/>
            <button v-on:click="selectPreviousRightArm()" class="prev-selector">&#9650;</button>
            <button v-on:click="selectNextRightArm()" class="next-selector">&#9660;</button>
          </div>
        </div>
        <div class="bottom-row">
          <div class="bottom part">
            <img v-bind:src="selectedRobot.base.src" title="bottom"/>
            <button v-on:click="selectPreviousBase()" class="prev-selector">&#9668;</button>
            <button v-on:click="selectNextBase()" class="next-selector">&#9658;</button>
          </div>
        </div>
        <div>
          <h1>Cart</h1>
          <table>
            <thead>
              <tr>
                <th>Robot</th>
                <th class="cost">Cost</th>
              </tr>
            </thead>
            <tbody>
              <tr v-for="(robot, index) in cart" :key="index">
                <td>
                  {{robot.head.title}}
                </td>
                <td class="cost">
                  {{robot.head.cost}}
                </td>
              </tr>
            </tbody>
          </table>
        </div>
      </div>
</template>

<script>
import avialableParts from '../data/parts';

function getPreviousValidIndex(index, lenght) {
  const deprecatedIndex = index - 1;
  return deprecatedIndex < 0 ? lenght -1 : deprecatedIndex;
}

function getNextalidIndex(index, lenght) {
  const incrementedIndex = index + 1;
  return incrementedIndex > lenght - 1 ? 0 : incrementedIndex; 
}

export default {
  name: 'RobotBuilder',
  data() {
    return {
        avialableParts,
        cart: [],
        indexHead: 0,
        indexRightArm: 0,
        indexLeftArm: 0,
        indexCenter: 0,
        indexBase: 0
    };
  },
  computed: {
    selectedRobot(){
      return{
        head: avialableParts.heads[this.indexHead],
        leftArm: avialableParts.arms[this.indexLeftArm],
        rightArm: avialableParts.arms[this.indexRightArm],
        center: avialableParts.torsos[this.indexCenter],
        base: avialableParts.bases[this.indexBase]
      }
    },
  },
  methods: {
    addToCart() {
      const robot = this.selectedRobot;
      const cost = robot.head.cost + 
        robot.leftArm.cost + 
        robot.rightArm.cost +
        robot.center.cost + 
        robot.base.cost;
      this.cart.push(Object.assign({}, robot, { cost }))
    },
    selectNextHead() {
      this.indexHead = getNextalidIndex(this.indexHead, avialableParts.heads.length)
    },
    selectPreviousHead() {
      this.indexHead = getPreviousValidIndex(this.indexHead, avialableParts.heads.length)
    },
    selectNextRightArm() {
      this.indexRightArm = getNextalidIndex(this.indexRightArm, avialableParts.arms.length)
    },
    selectPreviousRightArm() {
      this.indexRightArm = getPreviousValidIndex(this.indexRightArm, avialableParts.arms.length)
    },
    selectNextLeftArm() {
      this.indexLeftArm = getNextalidIndex(this.indexLeftArm, avialableParts.arms.length)
    },
    selectPreviousLeftArm() {
      this.indexLeftArm = getPreviousValidIndex(this.indexLeftArm, avialableParts.arms.length)
    },
    selectNextCenter() {
      this.indexCenter = getNextalidIndex(this.indexCenter, avialableParts.torsos.length)
    },
    selectPreviousCenter() {
      this.indexCenter = getPreviousValidIndex(this.indexCenter, avialableParts.torsos.length)
    },
    selectNextBase() {
      this.indexBase = getNextalidIndex(this.indexBase, avialableParts.torsos.length)
    },
    selectPreviousBase() {
      this.indexBase = getPreviousValidIndex(this.indexBase, avialableParts.torsos.length)
    }
  }
};
</script>

<style>
.part {
    position: relative;
    width:165px;
    height:165px;
    border: 3px solid #aaa;
  }
  .part img {
    width:165px;
  }
  .top-row {
    display:flex;
    justify-content: space-around;
  }
  .middle-row {
    display:flex;
    justify-content: center;
  }
  .bottom-row {
    display:flex;
    justify-content: space-around;
    border-top: none;
  }
  .head {
    border-bottom: none;
  }
  .left {
    border-right: none;
  }
  .right {
    border-left: none;
  }
  .left img {
    transform: rotate(-90deg);
  }
  .right img {
    transform: rotate(90deg);
  }
  .bottom {
    border-top: none;
  }
  .prev-selector {
    position: absolute;
    z-index:1;
    top: -3px;
    left: -28px;
    width: 25px;
    height: 171px;
  }
  .next-selector {
    position: absolute;
    z-index:1;
    top: -3px;
    right: -28px;
    width: 25px;
    height: 171px;
  }
  .center .prev-selector, .center .next-selector {
    opacity:0.8;
  }
  .left .prev-selector {
    top: -28px;
    left: -3px;
    width: 144px;
    height: 25px;
  }
  .left .next-selector {
    top: auto;
    bottom: -28px;
    left: -3px;
    width: 144px;
    height: 25px;
  }
  .right .prev-selector {
    top: -28px;
    left: 24px;
    width: 144px;
    height: 25px;
  }
  .right .next-selector {
    top: auto;
    bottom: -28px;
    left: 24px;
    width: 144px;
    height: 25px;
  }
  .right .next-selector {
    right: -3px;
  }
  .robot-name {
    position: absolute;
    top: -25px;
    text-align: center;
    width: 100%;
  }
  .sale {
    color: red;
  }
  .content {
    position: relative;
  }
  .add-to-cart {
    position: absolute;
    right: 30px;
    width: 220px;
    padding: 3px;
    font-size: 16px;
  }
</style>
