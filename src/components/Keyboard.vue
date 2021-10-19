<template>
  <div class="keyboard__container">
    <form class="keyboard__container--form">
      <p v-if="errors.length">
        <b>Please correct the following error(s):</b>
        <ul>
          <li v-for="(error, index) in errors" :key="index">{{ error }}</li>
        </ul>
      </p>
      <p>
        <label for="initialPoint"><strong>Initial starting point*:</strong></label>
        <input id="initialPoint" v-model="initialPoint" type="text" name="initialPoint" placeholder="4-0"/>
      </p>
      <p>
        <label for="commands"><strong>Commands*:</strong></label>
        <input id="commands" v-model="commands" type="text" name="commands" placeholder="FFRRFFFRL"/><br>
        <span>This field only allows you to enter the letters F, R or L.</span>
      </p>
      <p>
        <input
          class="button"
          type="button"
          value="Execute"
          @click="checkStartSequence()"
        />
      </p>
    </form>
  </div>
</template>

<script>
export default {
  data() {
    return {
      errors: [],
      initialPoint: "4-0",
      commands: ""
    };
  },
  methods: {
    /**
     * Validate form, if list errors is empty execute sequence
     */
    checkStartSequence(){
      this.errors = [];

      if (!this.initialPoint) {
        this.errors.push("Initial starting point required.");
      } else if (!this.validInitialPoint(this.initialPoint)) {
        this.errors.push('Wrong initial starting point format.');
      }

      if (!this.commands) {
        this.errors.push("Commands required.");
      } else if (!this.validCommands(this.commands)) {
        this.errors.push('Wrong commands format.');
      }

      if (!this.errors.length) {
        this.$emit('startSequence', { initialPoint: this.initialPoint, commands: this.commands })
      }
    },
    validInitialPoint(email) {
      const re = /^[0-9]-[0-9]$/;
      return re.test(email);
    },
    validCommands(email) {
      const re = /^[FRL]+$/;
      return re.test(email);
    }
  }
};
</script>

<style scoped>
.keyboard__container {
  background: #dfbe9f;
  border: 6px solid #ac7139;
  max-width: 600px;
  margin: 20px auto;
}

.keyboard__container--form {
  padding: 10px;
}

.button {
    background: #8f7a66;
    border-radius: 3px;
    padding: 0 20px;
    text-decoration: none;
    color: #f9f6f2;
    height: 40px;
    cursor: pointer;
}
</style>