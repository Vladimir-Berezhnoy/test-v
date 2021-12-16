<template>
<div>
  <form>
    <div v-if="!image">
      <h2>Select an image</h2>
      <input type="file" @change="onFileChange" required class="mgb">
    </div>
    <div v-else>
      <cropper
      class="cropper mgb"
      ref="image"
      :src="image"
      @change="change"
      ></cropper>
      <button @click="removeImage" class="mgb">Remove image</button>
    </div>
    <label class="db">
      Put the url
      <input type="text" v-model="url">
    </label>
    <label class="db">
      Choose a direction
      <select name="animation" v-model="animation">
        <option value="left" selected>from left to right</option>
        <option value="right">from right to left</option>
      </select>
    </label>
    <button type="submit" @click.prevent="upload()"> Submit </button>
  </form>
</div>
</template>

<script>
import { Cropper } from 'vue-advanced-cropper'
import 'vue-advanced-cropper/dist/style.css'
import pug from 'pug'
import template from '!!raw-loader!../template/index.pug'
import { saveAs } from 'file-saver'
export default {
  name: 'HelloWorld',
  components: {
    Cropper,
  },
  data() {
    return {
      url: 'https://www.',
      image: '',
      cropImage: '',
      animation: 'left',
    }
  },
  methods: {
    change({canvas}) {
      this.cropImage = canvas.toDataURL();
    },
    onFileChange(e) {
      var files = e.target.files || e.dataTransfer.files;
      if (!files.length)
        return;
      this.createImage(files[0]);
    },
    createImage(file) {
      var reader = new FileReader();
      reader.onload = (e) => {
        this.image = e.target.result;
      };
      reader.readAsDataURL(file);
    },
    removeImage() {
      this.image = '';
    },
    upload() {
      const result = pug.render(template, {url: this.url, href: this.cropImage, direction: this.animation});
      const blob = new Blob([result], {type: "text/plain;charset=utf-8"});
      saveAs(blob, "index.html");
    }
  }
}
</script>

<style scoped>
  .db {
    display: block;
    margin-bottom: 10px;
  }

  .mgb {
    margin-bottom: 10px;
  }
</style>
