<template>
  <div class="content">
    <button class="add-to-cart" @click="addToCart()">Add to cart</button>
    <div class="top-row">
      <div class="robot-name">
        {{ selectedRobot.head?.title }}
      </div>
      <PartSelector
        :parts="availableParts.heads"
        position="top"
        alt="head"
        @selectedPart="selectedHead"
      />
    </div>
    <div class="middle-row">
      <PartSelector
        :parts="availableParts.arms"
        position="left"
        alt="left arm"
        @selectedPart="selectedLeftArm"
      />

      <PartSelector
        :parts="availableParts.torsos"
        position="center"
        alt="torso"
        @selectedPart="selectedTorso"
      />

      <PartSelector
        :parts="availableParts.arms"
        position="right"
        alt="right arm"
        @selectedPart="selectedRightArm"
      />
    </div>
    <div class="bottom-row">
      <PartSelector
        :parts="availableParts.bases"
        position="bottom"
        alt="base"
        @selectedPart="selectedBottom"
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
import { computed, onMounted, ref } from 'vue';
import parts from '../data/parts';
import PartSelector from './PartSelector.vue';

onMounted(() => {
  console.log('mounted');
});

const availableParts = parts;
const selectedHeadIndex = ref(0);
const selectedLeftArmIndex = ref(0);
const selectedTorsoIndex = ref(0);
const selectedRightArmIndex = ref(0);
const selectedBaseIndex = ref(0);
const cart = ref([]);

const selectedRobot = computed(() => ({
  head: availableParts.heads[selectedHeadIndex.value],
  leftArm: availableParts.arms[selectedLeftArmIndex.value],
  torso: availableParts.torsos[selectedTorsoIndex.value],
  rightArm: availableParts.arms[selectedRightArmIndex.value],
  base: availableParts.bases[selectedBaseIndex.value],
}));

const addToCart = () => {
  const robot = selectedRobot.value;
  const cost =
    robot.head.cost + robot.leftArm.cost + robot.torso.cost + robot.rightArm.cost + robot.base.cost;
  cart.value.push({ ...robot, cost });
  console.log(cart.value.length);
};

// #region Part Selector Methods
const selectedHead = (index) => {
  console.log(index);
  console.log(selectedRobot.value);
  selectedHeadIndex.value = index;
};

const selectedLeftArm = (index) => {
  selectedLeftArmIndex.value = index;
};

const selectedTorso = (index) => {
  selectedTorsoIndex.value = index;
};

const selectedRightArm = (index) => {
  selectedRightArmIndex.value = index;
};

const selectedBottom = (index) => {
  selectedBaseIndex.value = index;
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

.add-to-cart {
  position: absolute;
  right: 30px;
  width: 220px;
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
