<script setup>
const files = ref([])
const imageUrls = ref([])
const isLoading = ref(false)

const uploadFile = (event) => {
  files.value = event.target.files
  for (const file of files.value) {
    const reader = new FileReader();

    reader.onload = function (event) {
      const imageUrl = event.target.result;
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
  <div class="flex flex-col justify-center max-w-3xl font-sans">
    <div class=" py-5 text-center bg-gray my-5">画像のアップロード</div>
    <div class="flex flex-wrap justify-between w-full">
      <label for="imgUpload" class="bg-gray w-32 h-32 m-2 flex items-center justify-center my-2">
        <img src="~/assets/upload.png" class="cursor-pointer w-8" alt="">
        <input id="imgUpload" type="file" multiple @change="uploadFile">
      </label>
      <div v-for="(imageUrl, index) in imageUrls " :key="imageUrl" class="m-2">
        <button @click="deleteImage(index)">×</button>
        <div class="relative w-32 h-32"><img :src="imageUrl" alt="" class="object-contain absolute w-full h-full"></div>
        <div class="flex justify-between w-full">
          <button @click="moveLeft(index)">←</button>
          <button @click="moveRight(index)">→</button>
        </div>
      </div>
    </div>

    <button :disabled="isLoading" @click="save" class="my-5 py-5">{{ isLoading ? 'Loading' : '保存' }}</button>
  </div>
</template>

<style>
#imgUpload {
  display: none;
}

.my-5 {
  margin: 1.25rem 0;
}

.mx-2 {
  margin: 0 0.5rem;
}

.absolute {
  position: absolute;
}

.object-contain {
  object-fit: contain;
}

.cursor-pointer {
  cursor: pointer;
}

.font-sans {
  font-family: ui-sans-serif, system-ui, -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, "Helvetica Neue", Arial, "Noto Sans", sans-serif, "Apple Color Emoji", "Segoe UI Emoji", "Segoe UI Symbol", "Noto Color Emoji";
}

.flex {
  display: flex;
}

.items-center {
  align-items: center;
}

.flex-col {
  flex-direction: column;
}

.flex-wrap {
  flex-wrap: wrap;
}

.justify-center {
  justify-content: center;
}

.justify-between {
  justify-content: between;
}

.mx-auto {
  margin: 0 auto;
}

.max-w-3xl {
  max-width: 48rem;
}

.m-2 {
  margin: 0.5rem;
}

.py-5 {
  padding: 1.25rem;
}

.w-full {
  width: 100%;
}

.h-full {
  height: 100%;
}


.w-8 {
  width: 2rem;
}

.w-32 {
  width: 8rem;
}

.h-8 {
  height: 2rem;
}

.h-32 {
  height: 8rem;
}

.text-center {
  text-align: center;
}

.bg-gray {
  background-color: rgba(128, 128, 128, 0.3);
}

.relative {
  position: relative;
}
</style>
