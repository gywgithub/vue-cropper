<template>
  <div id="app">
    <div>
      <div id="main" style="width: 600px;height:400px;"></div>
      <img id="image2" src="./assets/images/picture.jpg" alt="Picture"><br><br>
      <md-table md-card>
        <md-table-toolbar>
          <h1 class="md-title">Users</h1>
        </md-table-toolbar>

        <md-table-row>
          <md-table-head md-numeric>ID</md-table-head>
          <md-table-head>Name</md-table-head>
          <md-table-head>Email</md-table-head>
        </md-table-row>

        <md-table-row>
          <md-table-cell md-numeric>1</md-table-cell>
          <md-table-cell>Shawna Dubbin</md-table-cell>
          <md-table-cell>sdubbin0@geocities.com</md-table-cell>
        </md-table-row>

        <md-table-row>
          <md-table-cell md-numeric>2</md-table-cell>
          <md-table-cell>Odette Demageard</md-table-cell>
          <md-table-cell>odemageard1@spotify.com</md-table-cell>
        </md-table-row>

        <md-table-row>
          <md-table-cell md-numeric>3</md-table-cell>
          <md-table-cell>Vera Taleworth</md-table-cell>
          <md-table-cell>vtaleworth2@google.ca</md-table-cell>
        </md-table-row>
      </md-table><br><br>
      <img id="image3" src="./assets/images/picture.jpg" alt="Picture">
      <div class="open-container" v-show="openContainerShow">
        <md-button class="md-fab" @click="openCropContainer">
          <md-icon>crop_square</md-icon>
        </md-button>
      </div>  
    </div>

    <div class="div-container" v-show="containerShow">
      <md-button class="md-icon-button close" @click="cancelCrop">
        <md-icon>close</md-icon>
      </md-button>
      <!-- <canvas id="canvas1"></canvas> -->
      <!-- <img id="image" src="./assets/images/picture.jpg" alt="Picture"> -->
      <div style="border:1px solid blue;width:500px;height:400px;">
        <img :src="base64" id="image">
      </div>
      <p>
        Data:
        <span id="data"></span>
      </p>
      <p>
        Crop Box Data:
        <span id="cropBoxData" style="border:2px solid red;"></span>
      </p>
      <h3>Result</h3>
      <p>
        <md-button class="md-primary" id="button">Crop</md-button>
      </p>
      <div id="result" style="border:2px solid green;"></div>
    </div>

  </div>
</template>

<script>
import 'cropperjs/dist/cropper.css'
import Cropper from 'cropperjs'
import html2canvas from 'html2canvas'
import * as echarts from 'echarts'
export default {
  name: 'app',
  data () {
    return {
      base64: null,
      containerShow: false,
      openContainerShow: true
    }
  },
  methods: {
    initEcharts () {
      let myChart = echarts.init(document.getElementById('main'))
      let option = {
          title: {
              text: 'ECharts 入门示例'
          },
          tooltip: {},
          legend: {
              data:['销量']
          },
          xAxis: {
              data: ['衬衫','羊毛衫','雪纺衫','裤子','高跟鞋','袜子']
          },
          yAxis: {},
          series: [{
              name: '销量',
              type: 'bar',
              data: [5, 20, 36, 10, 10, 20]
          }]
      }
      myChart.setOption(option)
    },
    cancelCrop () {
      this.containerShow = false
      this.openContainerShow = true
    },
    crop () {
      console.log('crop')
    },
    openCropContainer () {
      let self = this
      self.openContainerShow = false
      let cropBoxData = document.querySelector('#cropBoxData')
      let data = document.querySelector('#data')
      let image = document.querySelector('#image')
      // document.documentElement.style.overflowY = 'hidden'
      // document.documentElement.style.overflowX = 'hidden'
      self.containerShow = true

      // let cropper = new Cropper(image, {
      //   ready: function (event) {
      //     cropper.zoomTo(1)
      //     console.warn(event)
      //   },

      //   crop: function (event) {
      //     console.log('crop')
      //     data.textContent = JSON.stringify(cropper.getData())
      //     cropBoxData.textContent = JSON.stringify(cropper.getCropBoxData())
      //     console.warn(event)
      //   },

      //   zoom: function (event) {
      //     if (event.detail.oldRatio === 1) {
      //       event.preventDefault()
      //     }
      //   }
      // })

      let body = document.body
      console.log(body)
      html2canvas(body, {
        // allowTaint: true,  
        // taintTest: false
      }).then(async function (canvas) {
        console.log(1)
        // document.body.appendChild(canvas)
        console.log(2)
        console.log(canvas)
        let base64 = await canvas.toDataURL('image/jpeg', 1)
        console.log(3)
        console.log(base64)
        self.base64 = base64
        let cropper = new Cropper(image, {
          ready: function (event) {
            console.log(4)
            // cropper.zoomTo(1)
            console.log(event)
          },

          crop: function (event) {
            data.textContent = JSON.stringify(cropper.getData())
            cropBoxData.textContent = JSON.stringify(cropper.getCropBoxData())
            console.log(event)
          },

          zoom: function (event) {
            if (event.detail.oldRatio === 1) {
              event.preventDefault()
            }
          }
        })
      })
    }
  },
  mounted () {
    this.initEcharts()
    // let self = this
    // let image = document.querySelector('#image')
    // let data = document.querySelector('#data')
    
    // let button = document.getElementById('button')
    // let result = document.getElementById('result')
    // let cropper = new Cropper(image, {
    //   ready: function (event) {
    //     cropper.zoomTo(1)
    //   },

    //   crop: function (event) {
    //     data.textContent = JSON.stringify(cropper.getData())
    //     cropBoxData.textContent = JSON.stringify(cropper.getCropBoxData())
    //   },

    //   zoom: function (event) {
    //     if (event.detail.oldRatio === 1) {
    //       event.preventDefault()
    //     }
    //   }
    // })

    // button.onclick = function () {
    //   result.innerHTML = ''
    //   let cropperCanvas = cropper.getCroppedCanvas()
    //   console.log(cropperCanvas)
    //   let img = cropperCanvas.toDataURL('image/jpeg')
    //   self.base64 = img
    //   result.appendChild(cropperCanvas)
    // }
  }
}
</script>
<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  width: 50%;
  margin: auto;
}
.container {
  max-width: 640px;
  margin: 20px auto;
}

img {
  max-width: 60%;
}

.div-container {
  background-color: #eee;
  position: fixed;
  z-index: 99;
  top: 0;
  bottom: 0;
  left: 0;
  right: 0;
  /*opacity: 0.4;*/
  /*border: 2px solid green;*/
}
#cancel {
  position: fixed;
  z-index: 1;
  left: 20px;
  bottom: 20px;
}
.open-button {
  position: absolute;
  z-index: 1;
  bottom: 20px;
  right: 20px; 
}
.open-container {
  width: 60px;
  height: 60px;
  position: fixed;
  /*z-index:1;*/
  bottom: 20px;
  right: 20px;
}
.close {
  position: fixed !important;
  top: 20px;
  right: 20px;
}
.crop-button {
  position: fixed !important;
  z-index:999;
  bottom: 20px;
  right: 20px;
}
</style>
