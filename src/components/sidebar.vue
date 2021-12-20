<template>
  <div class="absolute fl my24 mx24 py24 px24 bg-gray-faint round">
    <form id="params">
      <h4 class="txt-m txt-bold mb6">Choose a travel mode:</h4>
      <div class="mb12 mr12 toggle-group align-center">
        <label class="toggle-container">
          <input
            name="profile"
            type="radio"
            value="walking"
            v-model="profile"
            @click="show"
          />
          <div class="toggle toggle--active-null toggle--null">Walking</div>
        </label>
        <label class="toggle-container">
          <input
            name="profile"
            type="radio"
            value="cycling"
            v-model="profile"
          />
          <div class="toggle toggle--active-null toggle--null">Cycling</div>
        </label>
        <label class="toggle-container">
          <input
            name="profile"
            type="radio"
            value="driving"
            v-model="profile"
          />
          <div class="toggle toggle--active-null toggle--null">Driving</div>
        </label>
      </div>
      <h4 class="txt-m txt-bold mb6">Choose a maximum duration:</h4>
      <div class="mb12 mr12 toggle-group align-center">
        <label class="toggle-container">
          <input name="duration" type="radio" value="10" v-model="minutes" />
          <div class="toggle toggle--active-null toggle--null">10 min</div>
        </label>
        <label class="toggle-container">
          <input name="duration" type="radio" value="20" v-model="minutes" />
          <div class="toggle toggle--active-null toggle--null">20 min</div>
        </label>
        <label class="toggle-container">
          <input name="duration" type="radio" value="30" v-model="minutes" />
          <div class="toggle toggle--active-null toggle--null">30 min</div>
        </label>
      </div>
    </form>
  </div>
</template>
<script>
export default {
  data() {
    return { profile: 'walking', minutes: 10 }
  },

  methods: {
    show() {
      console.log('show')
    },
  },
}
</script>
<style scoped>
.absolute {
  position: absolute;
  top: 10px;
  left: 20px;
  z-index: 100;

  width: 300px;
}
@import url('https://api.mapbox.com/mapbox-assembly/v1.3.0/assembly.min.css');
</style>
