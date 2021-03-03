<template>
  <div v-scroll="onScroll">
    <v-row style="height: 70%">
      <canvas id="renderCanvas" style="height: 40%; width: 100%"></canvas>
    </v-row>
    <v-row style="height: 7000px">
      test
    </v-row>
  </div>
</template>

<script>
  import * as BABYLON from 'babylonjs';
  import * as BABYLON_MAT from "babylonjs-materials";
  import * as BabylonGUI from "babylonjs-gui"
  import 'babylonjs-loaders';
  import fabric from 'file-loader!@/assets/factory.babylon';
  import sky from 'file-loader!@/assets/lysGenerated_sunset.env';
  import { PointParticleEmitter } from 'babylonjs';

  BABYLON.ArcRotateCamera.prototype.spinTo = function (whichprop, targetval, speed) {
    var ease = new BABYLON.CubicEase();
    ease.setEasingMode(BABYLON.EasingFunction.EASINGMODE_EASEINOUT);
    BABYLON.Animation.CreateAndStartAnimation('at4', this, whichprop, speed, 120, this[whichprop], targetval, 0, ease);
  }

  export default {
    data() {
      return {
        offsetTop : 0,
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
        advancedTexture : null,
        fabric : fabric.replace("/", ""),
        sky : sky.replace("/", ""),
      };
    },
    methods: {
      onScroll (e) {
        this.mainCamera.position.y = 7 - ( e.target.scrollingElement.scrollTop / 100)
        this.mainCamera.rotation.x = 0 - ( e.target.scrollingElement.scrollTop / 5000)
        this.offsetTop = e.target.scrollingElement.scrollTop
      },
      logScene() {
        console.log(this.scene)
      },
      addSkyAndGround() {
        let skybox = BABYLON.Mesh.CreateBox("skyBox", 100.0, this.scene);

      },
      changeCamera(cameraName) {
        cameraName = ["Camera", "Camera.001"][cameraName]
        this.scene.setActiveCameraByName(cameraName)
        this.mainCamera = this.scene.getCameraByName(cameraName)
        //this.mainCamera.attachControl(this.canvas, true);
      },
      createSelector() {
        var selectBox = new BabylonGUI.SelectionPanel("sp");
        selectBox.width = 0.08;
        selectBox.height = 0.17;
        selectBox.background = "#ffffff";
        selectBox.alpha = 0.7
        selectBox.horizontalAlignment = BabylonGUI.Control.HORIZONTAL_ALIGNMENT_LEFT;
        selectBox.verticalAlignment = BabylonGUI.Control.VERTICAL_ALIGNMENT_TOP;
        
        this.advancedTexture.addControl(selectBox);
        var cameraGroup = new BabylonGUI.RadioGroup("Camera");
	      cameraGroup.addRadio("Camera 1", this.changeCamera, true);
        cameraGroup.addRadio("Camera 2", this.changeCamera, );
        selectBox.addGroup(cameraGroup);

      },
      createLabel(element, text) {
        let mesh = this.scene.getMeshByName(element)
        let rect1 = new BabylonGUI.Rectangle();
        rect1.width = 0.14;
        rect1.height = "30px";
        rect1.cornerRadius = 20;
        rect1.color = "Green";
        rect1.thickness = 4;
        rect1.background = "white";
        this.advancedTexture.addControl(rect1);
        rect1.linkWithMesh(mesh)  
        rect1.linkOffsetY = -60;

        let label = new BabylonGUI.TextBlock();
        label.text =  text;
        rect1.addControl(label);
      },
      async init() {
        return new Promise((resolve, reject) => {
          this.canvas = document.getElementById("renderCanvas")
          this.engine = new BABYLON.Engine(this.canvas, true)
          let wait = BABYLON.SceneLoader.Load("", this.fabric, this.engine, (scene) => {
            scene.setActiveCameraByName("Camera")
            this.mainCamera = scene.getCameraByName("Camera")
            //this.mainCamera.attachControl(this.canvas, true);
            scene.stopAllAnimations()
            scene.createDefaultSkybox(new BABYLON.CubeTexture(this.sky, scene), true, 100)
            this.advancedTexture = BabylonGUI.AdvancedDynamicTexture.CreateFullscreenUI("UI")
            this.engine.runRenderLoop(() => {
              scene.render()
            })
            this.scene = scene
            resolve("success")      
          })
        })
      }
      
    },
    async mounted() {
      let wait = await this.init()
      if (process.env.VUE_APP_DEBUG) {
        //this.scene.debugLayer.show();
      }
      this.createLabel('Cube.020', 'O2: 10Kg/h')
      this.createLabel('Door', 'Passed: 20 people')
      this.createLabel('Cube.038', 'CO2: 100Kg/h')
      this.createSelector()
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
  #renderCanvas {
    height: 70% !important;
  }
</style>