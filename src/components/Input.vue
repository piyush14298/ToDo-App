<template>
  <div>
    <form @submit.prevent="getuser">
      <input type="text" v-model="userId" name="title" placeholder="Enter ID to view your ToDos">
      <input type="submit" value="Submit" class="btn">
    </form>
  </div>
</template>

<script>
// import uuid from 'uuid';
export default {
  name: "getuser",
  data() {
    return {
      userId: ''
    }
  },
  methods: {
    getuser() {
      const userId = this.userId
      // Send up to parent
      this.$emit('getuser', userId);

      this.userId = '';
    }
  }
}
</script>

<style scoped>
  form {
    display: flex;
  }

  input[type="text"] {
    flex: 10;
    padding: 5px;
  }

  input[type="submit"] {
    flex: 2;
  }
</style>
