<template>
  <div class="about">
    <h1>{{ message }}</h1>
    <p>{{ body }}</p>
  </div>
</template>

<style>
</style>

<script>
export default {
  data: function() {
    return {
      message: "About us",
      body: "We love coffee and our mission is to provide the best supplies to get the most out of your beans."
    };
  },
  created: function() {},
  methods: {}
};
</script>