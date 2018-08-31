<template>
  <div id="app">
    
    <vue-upload-picker
      v-model="path1"
      data-title="上传文件"
      data-description="Choose image to upload."
      data-uri="a.php"
      :data-extension="['jpg', 'png', 'gif']"
      data-max-size="1024"></vue-upload-picker>

    <br>
    <vue-upload-picker
      v-model="path2"
      data-title="Choose image to upload"
      data-uri="a.php"
      data-label="path"></vue-upload-picker>

    <br>
    <vue-upload-picker
      v-model="path3"
      data-uri="a.php"
      data-title="Choose image to upload"></vue-upload-picker>

    <br>
    <vue-upload-picker
      data-title="Choose image to upload"
      data-uri="a.php"
      disabled></vue-upload-picker>
  </div>
</template>

<script>
import VueUploadPicker from "./components/VueUploadPicker";

export default {
  name: 'App',
  data() {
    return {
      path1: '',
      path2: {path: 'https://d9iyrkd8y2zpr.cloudfront.net/webapi-assets-test/resources/gifts/100426/msg/msg_1.png'},
      path3: [
        {path: 'https://d9iyrkd8y2zpr.cloudfront.net/webapi-assets-test/resources/gifts/100426/msg/msg_1.png'},
        {path: 'http://7xntdk.com1.z0.glb.clouddn.com/2.jpg'}
      ]
    };
  },
  components: {
    VueUploadPicker
  }
}
</script>

<style>
</style>
