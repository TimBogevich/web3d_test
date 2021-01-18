<template>
  <div>
    <v-slider v-model="value" step="1"></v-slider>
    <v-slider v-model="labelValue" step="1"></v-slider>
     <canvas id="renderCanvas" style="height: 100%; width: 100%"></canvas>
  </div>
</template>

<script>
  import * as BABYLON from 'babylonjs';
  import * as BABYLON_MAT from "babylonjs-materials";
    import * as BabylonGUI from "babylonjs-gui"
  import 'babylonjs-loaders';
  import fabric from 'file-loader!@/assets/fabric.babylon';

  BABYLON.ArcRotateCamera.prototype.spinTo = function (whichprop, targetval, speed) {
      var ease = new BABYLON.CubicEase();
      ease.setEasingMode(BABYLON.EasingFunction.EASINGMODE_EASEINOUT);
    BABYLON.Animation.CreateAndStartAnimation('at4', this, whichprop, speed, 120, this[whichprop], targetval, 0, ease);
  }

  export default {
    data() {
      return {
        value : 50,
        scene: null,
        ground : null,
        engine: null,
        canvas: null,
        mainCamera: null,
        light : null,
        shadowGenerator : null,
        label : null,
        labelValue : 50,
        fabric : fabric.replace("/", ""),
      };
    },
    methods: {
      logScene() {
        console.log(this.scene)
      },
      init() {
        const _this = this
        this.canvas = document.getElementById("renderCanvas")
        this.engine = new BABYLON.Engine(this.canvas, true)
        this.scene = new BABYLON.SceneLoader.Load("", this.fabric, this.engine, (scene) => {
          scene.debugLayer.show();
          scene.setActiveCameraByName("Camera")
          debugger
          this.mainCamera = scene.getCameraByName("Camera")
          this.engine.runRenderLoop(function () {
            scene.render()
          })
        })


      },


      
    },
    mounted() {
      console.log("test")
      this.init()
    },
    watch : {
      value(newValue, oldValue) {
        let camera = this.mainCamera
        debugger
        newValue = 1.5 + (newValue - 50) / 50
        camera.alpha = newValue
        //camera.spinTo("alpha", newValue, 900)
      },
      labelValue(newValue, oldValue) {
        this.label.text = `Lighthouse power: ${newValue}kw`;
      }
    } 
  }
</script>

<style lang="scss" scoped>

</style>