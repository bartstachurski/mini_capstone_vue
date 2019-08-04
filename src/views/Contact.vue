<template>
  <div class="contact">
    <h1>{{ message }}</h1>
    <a href="mailto:coffee@goodies.com?Subject=Hello%20From%The%20Web">Send Mail</a>
  </div>
</template>

<style>
</style>

<script>
export default {
  data: function() {
    return {
      message: "Contact Us!",
    };
  },
  created: function() {},
  methods: {}
};
</script>