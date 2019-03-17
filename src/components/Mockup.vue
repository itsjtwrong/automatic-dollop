<template>
<div class="hello">
  <img id="main-mockup" :src='mockup' alt="">
  <button type="button" v-on:click="gen" name="button">Generate Mockup</button>
  <select class="" name="size" v-model="size">
    <option value="S">S</option>
    <option value="M">M</option>
    <option value="L">L</option>
  </select>
  <br>
  Background: <input type="file" @change="processFile($event, 'back')"></input><br>
  Forground: <input type="file" @change="processFile($event, 'for')"></input>
</div>
</template>

<script>
import mergeImages from 'merge-images';
import resizeImage from 'resize-image';

export default {
  name: 'Mockup',
  methods: {
    gen: async function() {
      let that = this
      var img = new Image();
      img.src = that._data.forground.result
      var data = "";
      if (that._data.size === "S") {
        img.onload = function() {
          data = resizeImage.resize(img, 300, 200, resizeImage.JPEG);
          mergeImages([that._data.background.result, {
              src: data,
              x: 350,
              y: 200
            }])
            .then(b64 => that._data.mockup = b64);
        }
      } else if (that._data.size === "M") {
        img.onload = function() {
          data = resizeImage.resize(img, 400, 400, resizeImage.JPEG);
          mergeImages([that._data.background.result, {
              src: data,
              x: 310,
              y: 160
            }])
            .then(b64 => that._data.mockup = b64);
        }
      } else {
        img.onload = function() {
          data = resizeImage.resize(img, 450, 450, resizeImage.JPEG);
          mergeImages([that._data.background.result, {
              src: data,
              x: 280,
              y: 100
            }])
            .then(b64 => that._data.mockup = b64);
        }
      }
    },
    async processFile(event, place) {
      var reader = new FileReader();
      let theFile = event.target.files[0];
      await reader.readAsDataURL(theFile);
      console.log(reader.result)
      if(place === "back") {
          this._data.background = reader;
      } else {
        this._data.forground = reader;
      }

    }

  },
  data: function() {
    return {
      mockup: "",
      background: require("../assets/Tapestry-Free.jpg"),
      forground: require("../assets/image.jpg"),
      size: "L"
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}

ul {
  list-style-type: none;
  padding: 0;
}

li {
  display: inline-block;
  margin: 0 10px;
}

a {
  color: #42b983;
}
</style>
