  
<template>
<div>
  <Scene>
    <HemisphericLight>
      <property name="intensity" :float="2"></property>
    </HemisphericLight>
    <Camera type="arcRotate"></Camera>
    
    <HemisphericLight></HemisphericLight>
    <Asset :src="dummy3" :scaling="[10, 10, 10]">
      <Animation property="rotation.y" :duration="5" :end="-Math.PI * 2"></Animation>
    </Asset>
</Scene>
</div>
</template>

<script>
import dummy3 from 'file-loader!@/assets/mc-laren/source/McLaren.glb';

export default {

  data() {
    return {
      myScene: null,
      myEntity: null,
      myAssetRoot: null,
      dummy3,
    };
  },

  watch: {
    myScene() {
      // myScene is now available from the component
      // do something with it here or call a method to use it from here
    },

    myEntity() {
      // myEntity is now available from the component
      // do something with it here or call a method to use it from here
    },

    myBox() {
      // myBox is now available from the component
      // do something with it here or call a method to use it from here
    },
  },

}
</script>