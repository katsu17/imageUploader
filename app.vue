<script setup>
const files = ref([])
const imageUrls = ref([])
const isLoading = ref(false)

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

const deleteImage = (index) => {
  imageUrls.value.splice(index, 1)
}

const moveLeft = (index) => {
  if (index === 0) return
  const left = imageUrls.value[index - 1]
  imageUrls.value[index - 1] = imageUrls.value[index]
  imageUrls.value[index] = left
}

const moveRight = (index) => {
  if (index === (imageUrls.value.length - 1)) return
  const left = imageUrls.value[index + 1]
  imageUrls.value[index + 1] = imageUrls.value[index]
  imageUrls.value[index] = left
}

const save = async () => {
  isLoading.value = true
  const url = "https://httpbin.org/post"
  const data = { some: "fasdfa" }

  const response = await fetch(url, {
    method: 'POST', // *GET, POST, PUT, DELETE, etc.
    mode: 'cors', // no-cors, *cors, same-origin
    cache: 'no-cache', // *default, no-cache, reload, force-cache, only-if-cached
    credentials: 'same-origin', // include, *same-origin, omit
    headers: {
      'Content-Type': 'application/json'
      // 'Content-Type': 'application/x-www-form-urlencoded',
    },
    redirect: 'follow', // manual, *follow, error
    referrerPolicy: 'no-referrer', // no-referrer, *no-referrer-when-downgrade, origin, origin-when-cross-origin, same-origin, strict-origin, strict-origin-when-cross-origin, unsafe-url
    body: JSON.stringify(data) // 本文のデータ型は "Content-Type" ヘッダーと一致させる必要があります
  })
  const resopnseJson = await response.json(); // JSON のレスポンスをネイティブの JavaScript オブジェクトに解釈
  console.log(resopnseJson)
  isLoading.value = false
}

</script>

<template>
  <header>画像のアップロード</header>
  <label for="imgUpload">
    アップロード
    <input id="imgUpload" type="file" multiple @change="uploadFile">
  </label>

  <div class="images">
    <div v-for="(imageUrl, index) in imageUrls " :key="imageUrl">
      <img :src="imageUrl" alt="">
      <div class="buttons">
        <button @click="moveLeft(index)">←</button>
        <button @click="deleteImage(index)">×</button>
        <button @click="moveRight(index)">→</button>
      </div>
    </div>
  </div>

  <button :disabled="isLoading" @click="save">{{ isLoading ? 'Loading' : '保存' }}</button>
</template>

<style>
#imgUpload {
  display: none;
}

.incButton {
  width: 100px;
  height: 100px;
  border: 1px solid #000;
}

img {
  width: 140px;
  margin: 10px;
  border: solid 1px silver;
}

.buttons {
  display: flex;
  justify-content: space-between;
  width: 140px;
  margin: 10px;
}

.images {
  display: flex;
  flex-wrap: wrap;
  justify-content: space-between;
}

header {
  padding: 20px 0;
  background-color: rgba(128, 128, 128, 0.3);
  text-align: center;
}

input {
  margin: 20px 0;
}

.textCenter {
  text-align: center;
}

.relative {
  position: relative;
}
</style>
