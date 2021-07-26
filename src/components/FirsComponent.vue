<template>
  <div>
  {{ message }}
  </div>
</template>

<script>
export default {
  name: "FirstComponent",
  data() {
    return {
      message: "First component mounted"
    };
  }
};
</script>
