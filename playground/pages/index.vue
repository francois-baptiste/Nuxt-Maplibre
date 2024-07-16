<template>
  <div>
    <MapboxMap
      v-if="showMap"
      map-id="map2"
      style="top: 80px"
      :options="{
        style, // style URL
        center: [100.0, 0.0], // starting position [lng, lat]
        zoom: 3, // starting zoom
      }"
      :style="{
        height: `${height}px`
      }"
      @styleload="() => { console.log('style loaded') }"
    >
      <MapboxLayer
        v-if="enabled"
        :layer="layerRef"
        @click="showAlert"
      />
      <MapboxSource
        v-if="enabled"
        source-id="geojson"
        :source="source"
      />
      <MapboxDefaultMarker
        marker-id="customHTMLMarker"
        :lnglat="{ lng: 87, lat: 12 }"
        :options="{
          draggable: true
        }"
      >
        <template #marker>
          <button @click="showAlert">Map Button!</button>
        </template>
      </MapboxDefaultMarker>
      <MapboxDefaultMarker
        v-model:lnglat="lnglat"
        marker-id="marker1"
        :options="{
          draggable: true
        }"
        @dragend="() => { console.log('dragend') }"
      >
        <MapboxDefaultPopup
          popup-id="popup1"
          :lnglat="[0, 0]"
          :options="{
            closeOnClick: false
          }"
        >
          <h1 class="test">
            Hello World! {{ lnglat }}
            <button @click="showAlert">
              Click Me!
            </button>
          </h1>
        </MapboxDefaultPopup>
      </MapboxDefaultMarker>
      <TestMarker
        marker-id="marker2"
        :options="{
          lnglat
        }"
      />
      <TestControl />
      <MapboxGeocoder
        ref="geocoderRef"
        v-model="geocoderRes"
        position="top-left"
        @result="(result) => { console.log(result) }"
        :options="{
          version: 'v6'
        }"
      />
      <MapboxAttributionControl />
      <MapboxScaleControl />
      <MapboxNavigationControl />
      <MapboxFullscreenControl />
      <MapboxGeolocateControl />
    </MapboxMap>
    <NuxtLink to="/test">
      TEST
    </NuxtLink>
    <button @click="enabled = !enabled">
      Toggle Data
    </button>
    <button @click="changeData">
      Change Data
    </button>
    <button @click="changeColor">
      Change Color
    </button>
    <button @click="changeLngLat">
      Move Marker
    </button>
    <button @click="changeStyle">
      Random Style
    </button>
    <button @click="changeHeight">
      Resize Map
    </button>
    <button @click="toggleMap">
      Toggle Map
    </button>
  </div>
</template>

<<script setup>
import {ref} from "#imports"
const enabled = ref(true)
const remove = () => {
  enabled.value = !enabled.value
}
</script>
