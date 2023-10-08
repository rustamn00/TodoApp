<template>
  <button class="btn" @click="downloadProject">Download</button>
</template>

<script setup>
//   import { mapGetters } from 'vuex';
import { ref, onMounted } from "vue";
const title = ref("");

const downloadProject = () => {
  const exportedData = JSON.stringify(sessionStorage);

  const url = URL.createObjectURL(
    new Blob([exportedData], { type: "application/json" })
  );

  const link = document.createElement("a");
  link.href = url;
  link.download = `${title.value.length ? title.value : "empty"}.json`;
  link.click();

  URL.revokeObjectURL(url);
};

onMounted(() => {
  title.value = sessionStorage.getItem("name") || "";
});
</script>

<style scoped>
.btn {
  width: 200px;
  height: 40px;
  background-color: rgba(0, 0, 255, 0.726);
  border: none;
  border-radius: 8px;
  color: white;
}
</style>
