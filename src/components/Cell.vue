<template>
  <div class="cell__container">
    <label>{{x}}-{{y}}</label>
    <span v-if="cell.rover" :class="'rover '+cell.direction + ' '+cell.check">🛦</span>
  </div>
</template>

<script>
export default {
  props: {
    x: {
      type: Number,
      required: true
    },
    y: {
      type: Number,
      required: true
    },
    cell: {
      type: Object,
      required: true
    }
  }
};
</script>

<style scoped>
.cell__container {
  width: 106.25px;
  height: 106.25px;
  float: left;
  background: #dfbe9f;
  border: 6px solid #ac7139;
  margin: 0.5px;
}

.cell__container label {
  position: absolute;
}

.rover {
  font-size: 4rem;
  display: block;
  margin-left: auto;
  margin-right: auto;
  width: 70%;
}

.rover.check{
  color: #FAFAFA;
}

.rover.N {
  transform: rotate(0deg);
}

.rover.S {
  transform: rotate(180deg);
}

.rover.E {
  transform: rotate(90deg);
}

.rover.W {
  transform: rotate(270deg);
}

@media only screen and (max-width: 600px) {
  .cell__container {
    width: 80px;
    height: 80px;
    float: left;
    background: #dfbe9f;
    border: 6px solid #ac7139;
    margin: 0.5px;
  }

  .rover {
    font-size: 3rem;
    display: block;
    margin-left: auto;
    margin-right: auto;
    width: 65%;
  }
}
</style>