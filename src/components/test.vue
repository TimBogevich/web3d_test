<template>
  <div>
    <v-slider v-model="value" step="1"></v-slider>
     <canvas id="renderCanvas" style="height: 50%; width: 100%"></canvas>
  </div>
</template>

<script>
  import * as BABYLON from 'babylonjs';
  import * as BABYLON_MAT from "babylonjs-materials";
    import * as BabylonGUI from "babylonjs-gui"
  import 'babylonjs-loaders';
  import McLaren from 'file-loader!@/assets/untitled.glb';

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
        McLaren : McLaren.replace("/", ""),
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
        this.scene = new BABYLON.Scene(this.engine)
        this.scene.debugLayer.show();

        this.light = new BABYLON.DirectionalLight('light', new BABYLON.Vector3(1.5, -2.3, -1.6), this.scene)
        this.light.intensity = 10;

        // this.ground = BABYLON.Mesh.CreateGround("ground", 500, 500, 5, this.scene);
        // this.ground.receiveShadows = true

        var helper = this.scene.createDefaultEnvironment({
            enableGroundShadow: true
        });
        helper.setMainColor(BABYLON.Color3.Gray());
        helper.ground.position.y += 0;

        this.mainCamera = new BABYLON.ArcRotateCamera("ArcRotateCamera", 1.5, 1, 7, BABYLON.Vector3.Zero(), this.scene)

        this.engine.runRenderLoop(function () {
          _this.scene.render()
        })

        this.shadowGenerator = new BABYLON.ShadowGenerator(512, this.light)
        this.shadowGenerator.useBlurExponentialShadowMap = true;
        this.shadowGenerator.blurScale = 2;
        this.shadowGenerator.setDarkness(0.75);


        var advancedTexture = BabylonGUI.AdvancedDynamicTexture.CreateFullscreenUI("UI");




        BABYLON.SceneLoader.Append("", this.McLaren, this.scene, (scene) => {
          let car = scene.getMeshByUniqueID(69)
          //car.scaling = new BABYLON.Vector3(1, 1, 1);
          car.position.y = 0.2
          this.shadowGenerator.addShadowCaster(car);

        var rect1 = new BabylonGUI.Rectangle();
        rect1.width = 0.2;
        rect1.height = "30px";
        rect1.cornerRadius = 20;
        rect1.color = "Green";
        rect1.thickness = 4;
        rect1.background = "white";
        advancedTexture.addControl(rect1);
        rect1.linkWithMesh(car)  
        rect1.linkOffsetY = -8;

        var label = new BabylonGUI.TextBlock();
        label.text = "Power produced: 400 hp";
        rect1.addControl(label);

        });

      }
      
    },
    mounted() {
      console.log("test")
      this.init()
    },
    watch : {
      value(newValue, oldValue) {
        this.logScene()
        let camera = this.mainCamera
        newValue = 1.5 + (newValue - 50) / 10
        camera.alpha = newValue
      }
    } 
  }
</script>

<style lang="scss" scoped>

</style>