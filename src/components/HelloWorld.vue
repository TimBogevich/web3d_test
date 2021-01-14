  
<template>
<div>
  <Scene>
  <Box :position="[0, 0, 5]"></Box>
</Scene>
</div>
</template>

<script>
export default {

  data() {
    return {
      myScene: null,
      myEntity: null,
      myBox: null,
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