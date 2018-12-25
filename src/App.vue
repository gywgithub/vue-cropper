<template>
  <div id="app">
    <div class="container">
      <img id="image" src="./assets/images/picture3.jpeg">  
    </div>
    <md-button class="md-primary" @click="crop">crop</md-button>
    <div id="result"></div>
  </div>
</template>

<script>
import 'cropperjs/dist/cropper.css'
import Cropper from 'cropperjs'
export default {
  name: 'app',
  data () {
    return {
      cropper: null
    }
  },
  methods: {
    crop () {
      console.log('crop')
      let cropperCanvas = this.cropper.getCroppedCanvas()
      console.log(cropperCanvas)
      let result = document.getElementById('result')
      result.appendChild(cropperCanvas)
    },
    upload () {
      console.log('upload')
    }
  },
  async mounted () {
    let self = this
    const image = document.getElementById('image')
    await new Promise((resolve, reject) => {
      this.cropper = new Cropper(image, {
        aspectRatio: 16 / 9,
        ready (e) {
          resolve(true)
        },
        cropend (e) {
          console.log('e: ', e)
        },
        crop (e) {
        }
      })  
    })
    // this.cropper.clear()
    // console.log(this.cropper)
  }
}
</script>
<style>
.container {
  width: 500px;
  height: 400px;
}
#image {
  max-width: 100%;
  max-height: 100%;
}
</style>
