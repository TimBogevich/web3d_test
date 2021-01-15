<template>
  <div>
    <canvas id="the-canvas" style="height: 100%; width: 100%"></canvas>
  </div>
</template>

<script>
  import {Color3, Scene, ArcRotateCamera, DirectionalLight, MeshBuilder, Engine, SceneLoader, Mesh, Color4, Vector3, ShadowGenerator} from "babylonjs";
  import {AdvancedDynamicTexture, TextBlock, Rectangle, Ellipse, Line} from "babylonjs-gui"
  import {GridMaterial, ShadowOnlyMaterial} from "babylonjs-materials";
  import dummy3 from 'file-loader!@/assets/mc-laren/source/McLaren.glb';

  export default {
    data() {
      return {
        dummy3
      }
    },
    mounted() {
      const canvasa = document.querySelector("#the-canvas");
      var engine = new Engine(canvasa, true);
      const scene = this.CreateScene(engine, canvasa);
      engine.runRenderLoop(() => scene.render());
      console.log(dummy3)
    },
    methods: {
      CreateScene(engine, canvasa) {
            var scene = new Scene(engine)
            scene.clearColor = new Color4(0.66, 0.66, 0.66, 1);

            var camera = new ArcRotateCamera("Camera", -200, -200, 400, Vector3.Zero(), scene);
            camera.attachControl(canvasa, true)

            var light = new DirectionalLight('light', new Vector3(0, 1, 0.75), scene)
            light.intensity = 1;

            var ground = Mesh.CreatePlane('ground', 0, scene)
            ground.rotation.x = Math.PI / 2
            ground.material = new ShadowOnlyMaterial('mat', scene)
            ground.receiveShadows = true
            ground.position.y = 0;

            var groundPlaneMesh = MeshBuilder.CreateGround("groundPlaneMesh", { width: 400, height: 200, subdivisions: 1 }, scene);
            var gridMaterial = new GridMaterial("gridMaterial", scene);
            gridMaterial.gridRatio = 5;
            gridMaterial.majorUnitFrequency = 10;
            gridMaterial.backFaceCulling = false;
            gridMaterial.opacity = 0.50;
            gridMaterial.mainColor = new Color3(0.5, 0.5, 0.5);
            gridMaterial.lineColor = new Color3(0.5, 0.5, 0.5);
            groundPlaneMesh.material = gridMaterial;
            groundPlaneMesh.position.y = -30;
            groundPlaneMesh.position.z = 60;


            var sphere = MeshBuilder.CreateSphere("test",{})
            sphere.position.y = 25

            // GUI
            var advancedTexture = AdvancedDynamicTexture.CreateFullscreenUI("UI");

            var rect1 = new Rectangle();
            rect1.width = 0.2;
            rect1.height = "30px";
            rect1.cornerRadius = 20;
            rect1.color = "Green";
            rect1.thickness = 4;
            rect1.background = "white";
            advancedTexture.addControl(rect1);
            rect1.linkWithMesh(sphere)  
            rect1.linkOffsetY = -1;

            var label = new TextBlock();
            label.text = "Power produced: 400 hp";
            rect1.addControl(label);
            
            SceneLoader.Append("/", "McLaren.glb", scene, (scene) => {
              // Make the lighthouse bigger and place on the ground
              const lighthouse = scene.meshes[3];
              lighthouse.scaling = new Vector3(200, 200, 200);
              lighthouse.position.y = -10;
              lighthouse.position.z = 40;
              lighthouse.position.x = 0
            });



            return scene;
        }
    },
  }
</script>

<style lang="scss" scoped>

</style>