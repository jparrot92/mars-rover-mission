<template>
  <div>
    <Keyboard @startSequence="startSequence" />
    <div class="planet__container">
      <div class="planet-item" v-for="(row, indexRow) in grid" :key="indexRow">
        <cell
          v-for="(cell, indexCol) in row"
          :key="indexCol"
          :x="indexRow"
          :y="indexCol"
          :cell="cell"
        />
      </div>
    </div>
  </div>
</template>

<script>
import Keyboard from "./Keyboard";
import Cell from "./Cell";

export default {
  components: {
    Keyboard,
    Cell
  },
  data() {
    return {
      NUM_CELLS: 4,
      grid: [[]],
      direction: "N",
      positionY: 4,
      positionX: 0,
      obstacle: false
    };
  },
  created() {
    this.loadPlanet();
  },
  methods: {
    /**
     * Calculate the point of leaving the rover
     */
    calculateInitialPosition(initialPoint) {
      this.positionY = 4;
      this.positionX = 0;
      let positions = initialPoint.split("");
      this.positionY = parseInt(positions[0], 10);
      this.positionX = parseInt(positions[2], 10);

      this.isObstacle();
      if (this.obstacle) {
        alert("Invalid initial sequence!");
      }
    },
    /**
     * Load and reload planet
     */
    loadPlanet() {
      this.grid = [[]];
      this.direction = "N";
      this.obstacle = false;
      for (let i = 0; i <= this.NUM_CELLS; i++) {
        this.grid[i] = [];
        for (let j = 0; j <= this.NUM_CELLS; j++) {
          this.grid[i][j] = { rover: false, obstacle: false };
        }
      }
      this.grid[this.positionY][this.positionX] = {
        rover: true,
        direction: this.direction,
        check: "check"
      };
    },
    /**
     * Start rover sequence
     */
    async startSequence(sequence) {
      this.calculateInitialPosition(sequence.initialPoint);

      this.loadPlanet();

      let commands = sequence.commands.split("");

      for (let index = 0; index < commands.length; index++) {
        let command = commands[index];
        if (command === "F") {
          this.move();
          await this.waitSeconds();
          if (this.obstacle) {
            break;
          }
        } else if (command === "L") {
          this.direction = this.turnLeft();
          this.changeDirection();
          await this.waitSeconds();
        } else if (command === "R") {
          this.direction = this.turnRight();
          this.changeDirection();
          await this.waitSeconds();
        }
      }
    },
    /**
     * Move rover, and show the history movemnets rover
     */
    move() {
      let oldPositionY = this.positionY;
      let oldPositionX = this.positionX;

      if (this.direction === "N") {
        this.positionY = this.positionY - 1;
      } else if (this.direction === "S") {
        this.positionY = this.positionY + 1;
      } else if (this.direction === "E") {
        this.positionX = this.positionX + 1;
      } else if (this.direction === "W") {
        this.positionX = this.positionX - 1;
      }

      this.isObstacle();
      if (this.obstacle) {
        alert("An obstacle has been found. Aborted sequence!");
      } else {
        let grid = [...this.grid];

        let oldVal = {
          rover: true,
          direction: this.direction,
          check: ""
        };
        grid[oldPositionY][oldPositionX] = oldVal;

        let newVal = {
          rover: true,
          direction: this.direction,
          check: "check"
        };
        grid[this.positionY][this.positionX] = newVal;

        this.grid = grid;
      }
    },
    /**
     * Wait 1000ms to execute next movement rover
     */
    waitSeconds() {
      return new Promise(res => setTimeout(res, 1000));
    },
    /**
     * Calculate direction rover, if it goes to the left
     */
    turnLeft() {
      switch (this.direction) {
        case "N":
          return "W";
        case "W":
          return "S";
        case "S":
          return "E";
        case "E":
          return "N";
      }
    },
    /**
     * Calculate direction rover, if it goes to the right
     */
    turnRight() {
      switch (this.direction) {
        case "N":
          return "E";
        case "E":
          return "S";
        case "S":
          return "W";
        case "W":
          return "N";
      }
    },
    /**
     * Change direccion rover
     */
    changeDirection() {
      let grid = [...this.grid]

      let newVal = {
        rover: true,
        direction: this.direction,
        check: "check"
      };
      grid[this.positionY][this.positionX] = newVal

      this.grid = grid
    },
    /**
     * Check obstacles
     */
    isObstacle() {
      if (this.positionY === -1) {
        this.obstacle = true;
      } else if (this.positionX === -1) {
        this.obstacle = true;
      } else if (this.positionX > this.NUM_CELLS) {
        this.obstacle = true;
      } else if (this.positionY > this.NUM_CELLS) {
        this.obstacle = true;
      }
    }
  }
};
</script>

<style scoped>
.planet__container {
  max-width: 600px;
  margin: 20px auto;
}

.planet-item {
  display: block;
  clear: both;
}
</style>