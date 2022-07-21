<script setup>
const uploadFile = (event) => {
  files.value = event.target.files
  for (const file of files.value) {
    const reader = new FileReader();

    reader.onload = function (e) {
      const imageUrl = e.target.result;
      imageUrls.value.push(imageUrl)
    }
    reader.readAsDataURL(file);
  }
}

const files = ref([])
const imageUrls = ref([])

const deleteImage = (index) => {
  imageUrls.value.splice(index, 1)
}

</script>

<template>
  <div>
    <div>商品写真</div>
    <input type="file" multiple @change="uploadFile">
    <div v-for="(imageUrl, index) in imageUrls " id="preview" :key="imageUrl">
      <img :src="imageUrl" alt="">
      <button @click="deleteImage(index)">×</button>
    </div>
  </div>
</template>

<style>
.incButton {
  width: 100px;
  height: 100px;
  border: 1px solid #000;
}

#preview img {
  width: 200px;
  margin: 10px;
  border: solid 1px silver;
}
</style>
