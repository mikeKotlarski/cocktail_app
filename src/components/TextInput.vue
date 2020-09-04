<template>
  <div>
    <input type="text" placeholder="eg., Gin and Tonic" v-model="userInput" @keyup.enter="submit">
  </div>
</template>

<script>
export default {
  name: "UsrMsg",
  data() {
    return {
      userInput: ""
    };
  },
  methods: {
    submit: function() {
      this.$emit("inputData", this.userInput);
      this.userInput = "";
    }
  }
};
</script>

<style scoped>
input[type="text"],
select {
  width: 100%;
  padding: 12px 20px;
  margin: 8px 0;
  display: inline-block;
  border: 1px solid;
  border-radius: 4px;
  box-sizing: border-box;
}
</style>
