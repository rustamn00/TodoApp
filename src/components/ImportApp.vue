<template>
  <form class="form">
    <div class="">
      <input id="file" class="" type="file" @change="handleFile" />
    </div>
    <button class="btn" @click="importFile">Upload</button>
  </form>
</template>

<script>
import { defineComponent } from "vue";
export default defineComponent({
  data() {
    return {
      selectedFile: "Выберите файл",
      file: null,
    };
  },
  methods: {
    handleFile(event) {
      const input = event.target;
      if (input.files) {
        this.file = input.files[0];
        this.selectedFile = this.file.name;
      } else {
        this.file = null;
        this.selectedFile = "Выберите файл";
      }
    },
    importFile(e) {
      e.preventDefault();

      if (!this.file) {
        this.selectedFile = "Файл не выбран.";
        return;
      }
      const reader = new FileReader();

      reader.onload = function (event) {
        const fileContent = event.target?.result;

        if (typeof fileContent === "string") {
          const jsonData = JSON.parse(fileContent);
          for (var key in jsonData) {
            sessionStorage.setItem(key, jsonData[key]);
          }
        }
      };
      reader.readAsText(this.file);
      window.location.reload();
    },
  },
});
</script>

<style scoped>
.btn {
  margin-top: 10px;
  width: 200px;
  height: 40px;
  background-color: rgba(0, 0, 255, 0.726);
  border: none;
  border-radius: 8px;
  color: white;
}
</style>
