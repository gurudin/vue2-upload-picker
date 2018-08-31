<template>
<div class="vue-upload-picker">
  <div class="up-Button up-Button--compound" :disabled="isDisabled" @click="transfer">
    <span class="up-Button-label">{{dataTitle}}</span>
    <span class="up-Button-description">{{dataDescription}}</span>

    <label class="up-img" v-for="files in filesData">
      <img :src="files[defaultLabel]">
    </label>
  </div>

  <input type="hidden" :value="value">
  <input type="file" @change="upload" ref="file">
</div>
</template>

<script>
const lang = {
  'en': {
    'Invalid file extension': 'Invalid file extension!',
    'Exceeded size limit': 'Exceeded size limit!',
  },
  'zh': {
    'Invalid file extension': '格式不正确!',
    'Exceeded size limit': '大小超过限制',
  },
};

export default {
  props: {
    value: {
      type: null,
      required: false
    },
    dataTitle: {
      type: String,
      required: true
    },
    dataUri: {
      type: String,
      required: true
    },
    dataDescription: {
      type: String,
      required: false
    },
    dataLabel: {
      type: String,
      required: false
    },
    dataExtension: {
      type: Array,
      required: false
    },
    dataLang: {
      type: String,
      required: false
    },
    dataMaxSize: { // kb
      type: null,
      required: false
    },
    disabled: null
  },
  data() {
    return {
      defaultLabel: 'path',
      defaultLang: 'zh',
    };
  },
  computed: {
    isDisabled() {
      return typeof this.disabled == "undefined" ? false : true;
    },
    filesData() {
      var files = [];
      var tmpPath = {};

      this.defaultLabel = this.dataLabel ? this.dataLabel : 'path';
      if (typeof this.value == 'object') {
        if (this.value.length > 0) {
          files = this.value;
        } else {
          files.push(this.value);
        }
      } else {
        if (this.value) {
          tmpPath[this.defaultLabel] = this.value;
          files.push(tmpPath);
        }
      }

      return files;
    }
  },
  methods: {
    transfer(event) {
      this.$refs.file.click();
    },
    t(key) {
      return lang[this.defaultLang][key];
    },
    clearFile(fileObj) {
      fileObj.value = ''
    },
    upload(event) {
      var file = event.target.files[0];
      if (!file) return false;
      
      let ext   = file.name.toLowerCase().substr(file.name.lastIndexOf(".") + 1);
      var _this = this;

      // Check extension.
      if (_this.dataExtension) {
        if (_this.dataExtension.indexOf(ext) == -1) {
          alert(_this.t('Invalid file extension'));
          _this.clearFile(event.target);
          return false;
        }
      }

      // Check file size
      if (this.dataMaxSize) {
        if (file.size > this.dataMaxSize * 1024) {
          alert(_this.t('Exceeded size limit'));
          _this.clearFile(event.target);
          return false;
        }
      }

      // Upload
      var data = new FormData();
      data.append('file', file);
      
      var url = "http://gurudin.test/vue2-upload-picker/test.php";
      var xhr = new XMLHttpRequest();
      xhr.open("post", url, true);
      xhr.onload = uploadComplete; //请求完成
      xhr.onerror =  uploadFailed; //请求失败
      // xhr.upload.onprogress = progressFunction;
      xhr.send(data);

      function uploadComplete(evt) {
        console.log(evt.target);
        
        if (evt.target.status == 200) {

        }
      }
      
      function uploadFailed(evt) {
        console.log(evt);
      }
    }
  },
  created() {
    this.defaultLang = this.dataLang ? this.dataLang : this.defaultLang;
    // console.log(typeof this.value);
  }
};
</script>

<style>
.up-Button:disabled {
  opacity: 0.65;
  pointer-events: none;
}
.up-Button .up-img {
  margin-top: 7px;
  display: block;
}
.up-Button .up-img img {
  width: 100%;
  height: 100%;
  border-radius: 2px;
}
.up-Button.up-Button--compound {
  position: relative;
  display: block;
  height: auto;
  max-width: 280px;
  min-height: 72px;
  padding: 20px;
  border-radius: 5px;
}
.up-Button {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
  box-shadow: none;
  font-family: Segoe UI WestEuropean, Segoe UI, -apple-system,
    BlinkMacSystemFont, Roboto, Helvetica Neue, sans-serif;
  -webkit-font-smoothing: antialiased;
  color: #333;
  font-size: 14px;
  font-weight: 400;
  background-color: #f4f4f4;
  border: 1px solid #f4f4f4;
  cursor: pointer;
  display: inline-block;
  height: 32px;
  min-width: 80px;
  padding: 4px 20px 6px;
}
.up-Button:hover {
  background-color: #eaeaea;
  border-color: #eaeaea;
}
.up-Button.up-Button--compound .up-Button-label {
  display: block;
  font-weight: 600;
  position: relative;
  text-align: left;
  margin-top: -5px;
}
.up-Button-label {
  color: #333;
  font-weight: 600;
  font-size: 15px;
}
.up-Button.up-Button--compound .up-Button-description {
  color: #666;
  display: block;
  font-weight: 400;
  font-size: 12px;
  position: relative;
  text-align: left;
  top: 3px;
}
</style>
