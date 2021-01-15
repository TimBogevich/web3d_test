<template>
  <div>
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
        scene: null,
        engine: null,
        canvas: null,
        mainCamera: null,
        dummy3 : dummy3.replace("/", ""),
      };
    },
    methods: {
      init() {
        const _this = this
        this.canvas = document.getElementById("renderCanvas")
        this.engine = new BABYLON.Engine(this.canvas, true)
        this.scene = new BABYLON.Scene(this.engine)
        this.scene.clearColor = new BABYLON.Color4(0.66, 0.66, 0.66, 1);

        this.mainCamera = new BABYLON.ArcRotateCamera("ArcRotateCamera", 1.5, 1.3, 20, BABYLON.Vector3.Zero(), this.scene)
        this.mainCamera.attachControl(this.canvas, true)
        this.scene.beforeRender = function () {

        }
        this.engine.runRenderLoop(function () {
          _this.scene.render()
        })
        BABYLON.SceneLoader.Append("", this.dummy3, this.scene, (scene) => {
          const lighthouse = scene.meshes[3];
            lighthouse.scaling = new Vector3(600, 600, 600);
            lighthouse.position.y = -10;
            lighthouse.position.z = 40;
            lighthouse.position.x = 0
        });

      }
      
    },
    mounted() {
      console.log("test")
      this.init()
    },
  }
</script>

<style lang="scss" scoped>

</style>