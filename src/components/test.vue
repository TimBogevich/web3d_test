<template>
  <div>
    <v-slider v-model="value" step="1"></v-slider>
     <canvas id="renderCanvas" style="height: 100%; width: 100%"></canvas>
  </div>
</template>

<script>
  import * as BABYLON from 'babylonjs';
  import 'babylonjs-loaders';
  import dummy3 from 'file-loader!@/assets/mc-laren/source/McLaren.glb';

  export default {
    data() {
      return {
        value : 50,
        scene: null,
        engine: null,
        canvas: null,
        mainCamera: null,
        light : null,

        dummy3 : dummy3.replace("/", ""),
      };
    },
    methods: {
      init() {
        const _this = this
        this.canvas = document.getElementById("renderCanvas")
        this.engine = new BABYLON.Engine(this.canvas, true)
        this.scene = new BABYLON.Scene(this.engine)
        this.scene.clearColor = new BABYLON.Color4(0.8, 0.8, 0.156, 0.9);

        this.mainCamera = new BABYLON.ArcRotateCamera("ArcRotateCamera", 1.5, 1.3, 2, BABYLON.Vector3.Zero(), this.scene)
        this.mainCamera.attachControl(this.canvas, true)
        this.scene.beforeRender = function () {


        this.light = new BABYLON.DirectionalLight('light', new BABYLON.Vector3(1.5, -1.3, -1.6), this.scene)
        this.light.intensity = 10;

        }
        this.engine.runRenderLoop(function () {
          _this.scene.render()
        })
        BABYLON.SceneLoader.Append("", this.dummy3, this.scene, (scene) => {

        });

      }
      
    },
    mounted() {
      console.log("test")
      this.init()
    },
    watch : {
      value(newValue, oldValue) {
        let camera = this.mainCamera
        newValue = 2 + (newValue - 50) / 10
        camera.setPosition(new BABYLON.Vector3(1.5, 1.3, newValue));
        console.log(newValue)
      }
    } 
  }
</script>

<style lang="scss" scoped>

</style>