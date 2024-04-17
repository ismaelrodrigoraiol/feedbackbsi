<script setup>

const message = defineProps(['msg'])

</script>

<template>

  <div class="message">
      <p>{{ message.msg }}</p>
  </div>
  
</template>


<style scoped>
.message {
  width: 95%;
  text-align: center;
  color: #004085;
  background-color: #cce5ff;
  border: 2px solid #88daff;
  border-radius: 5px;
  padding: 10px;
  max-width: 500px;
  margin: 30px auto;
}

</style>
