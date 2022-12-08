<template>
  <div class="container">
    <Drogue>
      <DrogueItem>
        开门
      </DrogueItem>
      <DrogueItem type="info">
        续单
      </DrogueItem>
      <DrogueItem type="danger">
        离店
      </DrogueItem>
    </Drogue>
  </div>
</template>
<script setup>
import Card from './components/Card.vue'
import Drogue from './components/Drogue/index.vue'
import DrogueItem from './components/Drogue/DrogueItem.vue'
</script>
<style scoped>
.container {
  height: 3000px;
}
</style>