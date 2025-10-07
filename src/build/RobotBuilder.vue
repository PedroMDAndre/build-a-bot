<template>
  <div class="content">
    <div class="preview">
      <div class="preview-content">
        <div class="top-row">
          <img :src="selectedRobot.head.imageUrl" alt="head" />
        </div>
        <div class="middle-row">
          <img :src="selectedRobot.leftArm.imageUrl" class="rotate-left" alt="left arm" />
          <img :src="selectedRobot.torso.imageUrl" alt="torso" />
          <img :src="selectedRobot.rightArm.imageUrl" class="rotate-right" alt="right arm" />
        </div>
        <div class="bottom-row">
          <img :src="selectedRobot.base.imageUrl" alt="base" />
        </div>
      </div>
      <button class="add-to-cart" @click="addToCart()">Add to Cart</button>
    </div>
    <div class="top-row">
      <div class="robot-name">
        {{ selectedRobot.head?.title }}
      </div>
      <PartSelector
        :parts="availableParts.heads"
        position="top"
        alt="head"
        @partSelected="selectedHead"
      />
    </div>
    <div class="middle-row">
      <PartSelector
        :parts="availableParts.arms"
        position="left"
        alt="left arm"
        @partSelected="selectedLeftArm"
      />

      <PartSelector
        :parts="availableParts.torsos"
        position="center"
        alt="torso"
        @partSelected="selectedTorso"
      />

      <PartSelector
        :parts="availableParts.arms"
        position="right"
        alt="right arm"
        @partSelected="selectedRightArm"
      />
    </div>
    <div class="bottom-row">
      <PartSelector
        :parts="availableParts.bases"
        position="bottom"
        alt="base"
        @partSelected="selectedBottom"
      />
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
          <td>{{ robot.head.title }}</td>
          <td class="cost">{{ toCurrency(robot.cost) }}</td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script setup>
import toCurrency from '@/shared/formatter';
import { onMounted, ref } from 'vue';
import parts from '../data/parts';
import PartSelector from './PartSelector.vue';

onMounted(() => {
  console.log('mounted');
});

const availableParts = parts;
const cart = ref([]);

const selectedRobot = ref({
  head: {},
  leftArm: {},
  torso: {},
  rightArm: {},
  base: {},
});

const addToCart = () => {
  const robot = selectedRobot.value;
  const cost =
    robot.head.cost + robot.leftArm.cost + robot.torso.cost + robot.rightArm.cost + robot.base.cost;
  cart.value.push({ ...robot, cost });
  console.log(cart.value.length);
};

// #region Part Selector Methods
const selectedHead = (part) => {
  selectedRobot.value.head = part;
};

const selectedLeftArm = (part) => {
  selectedRobot.value.leftArm = part;
};

const selectedTorso = (part) => {
  selectedRobot.value.torso = part;
};

const selectedRightArm = (part) => {
  selectedRobot.value.rightArm = part;
};

const selectedBottom = (part) => {
  selectedRobot.value.base = part;
};

// #endregion
</script>

<style lang="scss" scoped>
.top-row {
  display: flex;
  justify-content: space-around;
}

.middle-row {
  display: flex;
  justify-content: center;
}

.bottom-row {
  display: flex;
  justify-content: space-around;
  border-top: none;
}

.robot-name {
  position: absolute;
  top: -25px;
  text-align: center;
  width: 100%;
}

.content {
  position: relative;
}

.preview {
  position: absolute;
  top: -20px;
  right: 0;
  width: 310px;
  height: 310px;
  padding: 5px;
}

.preview-content {
  border: 1px solid #999;
}

.preview img {
  width: 70px;
  height: 70px;
}

.rotate-right {
  transform: rotate(90deg);
}

.rotate-left {
  transform: rotate(-90deg);
}

.add-to-cart {
  position: absolute;
  width: 310px;
  padding: 3px;
  font-size: 16px;
}

td,
th {
  text-align: left;
  padding: 5px;
  padding-right: 20px;
}
</style>
