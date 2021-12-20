<template>
  <div style="height: 100%; position: relative; width: 100%; text-align: left">
    <SideBar></SideBar>
    <div ref="basicMapbox" :style="mapSize"></div>
  </div>
</template>
<script>
import mapboxgl from 'mapbox-gl'
import SideBar from './sidebar.vue'
export default {
  props: {
    mapWidth: {
      type: String,
    },
    mapHeight: {
      type: String,
    },
  },

  components: {
    SideBar,
  },
  data() {
    return {}
  },
  //   load(){
  //       console.log("loaded!")
  //   },
  mounted() {
    this.init()
  },
  methods: {
    // 初始化
    init() {
      mapboxgl.accessToken =
        'pk.eyJ1IjoiZnJhbmtkZXY1MjciLCJhIjoiY2tsN3FscjQ4MjdhYzJvdDd3YWllNndubCJ9.sFEi7deMi2-xsfBl8qX-gA'
      const map = new mapboxgl.Map({
        container: this.$refs.basicMapbox, //指向某个DIV
        style: 'mapbox://styles/mapbox/streets-v9',
        center: [120.106164, 30.880402], //[longitude经度,latitude纬度]
        zoom: 15, // starting zoom
      })
      //   console.log(map)
      map.addControl(new mapboxgl.FullscreenControl())

      const nav = new mapboxgl.NavigationControl({
        visualizePitch: true,
        showCompass: true,
      })
      map.addControl(nav, 'top-right')

      // Create a popup
      const popup = new mapboxgl.Popup({ closeOnClick: false })
      popup
        .setLngLat([120.106164, 30.880402])
        .setHTML('<h1>家家爱生活服务平台</h1>')
        .addTo(map)
      // Set an event listener that will fire
      // any time the popup is opened
      popup.on('click', () => {
        console.log('popup was opened')
      })

      //   map.scrollZoom.enable()
      //   map.DragPanHandler.disable()

      // Create a new marker.
      //   mapboxgl.Marker({
      //     color: '#FFFFFF',
      //     draggable: true,
      //   })
      //     .setLngLat([120.106164, 30.880402])
      //     .addTo(map)

      // Add markers to the map.
      // for (const marker of geojson.features) {
      // // Create a DOM element for each marker.
      // const el = document.createElement('div');
      // const width = marker.properties.iconSize[0];
      // const height = marker.properties.iconSize[1];
      // el.className = 'marker';
      // el.style.backgroundImage = `url(https://placekitten.com/g/${width}/${height}/)`;
      // el.style.width = `${width}px`;
      // el.style.height = `${height}px`;
      // el.style.backgroundSize = '100%';

      // el.addEventListener('click', () => {
      // window.alert(marker.properties.message);
      // });

      // // Add markers to the map.
      // new
      const geojson = {
        type: 'FeatureCollection',
        features: [
          {
            type: 'Feature',
            properties: {
              message: 'Foo',
              iconSize: [60, 60],
            },
            geometry: {
              type: 'Point',
              coordinates: [120.113084, 30.878],
            },
          },
          {
            type: 'Feature',
            properties: {
              message: 'Bar',
              iconSize: [50, 50],
            },
            geometry: {
              type: 'Point',
              coordinates: [120.098857, 30.879702],
            },
          },
          {
            type: 'Feature',
            properties: {
              message: 'Baz',
              iconSize: [40, 40],
            },
            geometry: {
              type: 'Point',
              coordinates: [120.106604, 30.885613],
            },
          },
        ],
      }
      for (const marker of geojson.features) {
        const el = document.createElement('div')
        const width = marker.properties.iconSize[0]
        const height = marker.properties.iconSize[1]
        el.className = 'marker'
        el.style.backgroundImage = `url(https://placekitten.com/g/${width}/${height}/)`
        el.style.width = `${width}px`
        el.style.height = `${height}px`
        el.style.backgroundSize = '100%'
        new mapboxgl.Marker(el)
          .setLngLat(marker.geometry.coordinates)
          .addTo(map)
      }

      // const urlBase="https://api.mapbox.com/isochrone/v1/mapbox/"
      // const profile="driving"
      // const lon="120.106604"
      // const lat="30.885613"
      // const minutes="5"

      const urlBase = 'https://api.mapbox.com/isochrone/v1/mapbox/'
      const lon = 120.106164
      const lat = 30.880402
      const profile = 'cycling' // Set the default routing profile
      const minutes = 5 // Set the default duration
      // Create a function that sets up the Isochrone API query then makes an fetch call
      async function getIso() {
        const query = await fetch(
          `${urlBase}${profile}/${lon},${lat}?contours_minutes=${minutes}&polygons=true&access_token=${mapboxgl.accessToken}`,
          { method: 'GET' }
        )
        const data = await query.json()
        // console.log(data)
        // console.log(data)
        map.getSource('iso').setData(data);
      }

     

      map.on('load', () => {
        // When the map loads, add the source and layer
        map.addSource('iso', {
          type: 'geojson',
          data: {
            type: 'FeatureCollection',
            features: [],
          },
        })

        map.addLayer({
          id: 'isoLayer',
          type: 'fill',
          // Use "iso" as the data source for this layer
          source: 'iso',
          layout: {},
          paint: {
            // The fill color for the layer is set to a light purple
            'fill-color': '#5a3fc0',
            'fill-opacity': 0.3,
          },
        })

        // Make the API call
        getIso()
        // map.getSource('iso').setData(data);
      })
    },

    // Add the control to the map.
  },
  computed: {
    mapSize() {
      let styleObj = {
        width: this.mapWidth,
        height: this.mapHeight,
      }
      return styleObj
    },
  },
}
</script>
<style>
@import url('https://api.tiles.mapbox.com/mapbox-gl-js/v0.44.2/mapbox-gl.css');
.marker {
  display: block;
  border: none;
  border-radius: 50%;
  cursor: pointer;
  padding: 0;
}
</style>
