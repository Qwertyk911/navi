<template>
    <div id="mapContainer">
          <div class="kol-flex">
        <div class="kol-flex-child"></div>
        <div class="kol-flex-child paddingTopLeft leaflet-bar leaflet-control-container " onclick = "event.stopPropagation()">
            <button class="kol-btn" @click="addMark();">Добавить маркер</button>
            <button class="kol-btn" @click="centerMark();">Выравнивание по центру</button>
            <button class="kol-btn" @click="removeMark();">Удалить маркер</button>
        </div>
        <div class="kol-flex-child"></div>

    </div>
    </div>
</template>

<script>
import L from 'leaflet'
import 'leaflet/dist/leaflet.css'
// Импортирует изображение маркера чтобы оно было корректно установлено
import icon from 'leaflet/dist/images/marker-icon.png';
import iconShadow from 'leaflet/dist/images/marker-shadow.png';

let latitude = 54.9179;
let longitude = 37.4265;
let stackMark = [];
let marker = '';

let DefaultIcon = L.icon({
    iconUrl: icon,
    shadowUrl: iconShadow
});
L.Marker.prototype.options.icon = DefaultIcon;

export default {
  name: 'MapComponent',
  data () {
    return {
      centerCoordinates: [54.9179, 37.4265],
      map: null,
      tileLayer: null
    }
  },
  mounted() {
    console.log("TEST")
    this.initMap();
    this.map.on('click', (event) => {
      this.mapClick(event);
    })
  },
  methods: {
    initMap() {
      this.map = L.map('mapContainer').setView(this.centerCoordinates, 13);
      this.tileLayer = L.tileLayer(
        'http://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png',
        //"https://api.mapbox.com/styles/v1/{id}/tiles/{z}/{x}/{y}?access_token={accessToken}"
        {
          attribution: 'testMap',
          maxZoom: 18,
          id: "mapbox/satellite-v9",
          //accessToken:"pk.eyJ1IjoiaXZhbi1rb2xlcyIsImEiOiJja3lmejNodmQxbm56Mm9wYjFwY2IwaGRmIn0.VWdZD_HnDx2qm5qVr6tzzg",
        }
      );
      this.tileLayer.addTo(this.map);
    },
    mapClick(event) {
      //this.createMarker(event.latlng);
                 marker = L.marker(event.latlng);
            //marker.bindPopup().openPopup("Координаты точки: ", e.latlng);
            marker.addTo(this.map);
            stackMark.push(marker);
           latitude = event.latlng.lat;
           longitude = event.latlng.lng;
    },
    createMarker(latlng) {
      L.marker(latlng).addTo(this.map);
    },
            removeMark() {
            // remove a marker         
            this.map.removeLayer(stackMark.pop());
        },
        centerMark() {
            const position = [latitude, longitude];
            this.map.flyTo(position, 10);
        },
        getRandomArbitrary(min, max) {
            return Math.random() * (max - min) + min;
        },
        addMark() {
            // Creating a marker
            // rand latitude and longitude for mark
            latitude = this.getRandomArbitrary(-90, 90);
            longitude = this.getRandomArbitrary(-180, 180);
            marker = L.marker([latitude, longitude]);
            stackMark.push(marker);
            // Adding marker to the map
            marker.addTo(this.map);
            alert("Маркер с координатами " + latitude + " " + longitude + " добавлен");
        }
  },
}
</script>

<style scoped>
#mapContainer {
  cursor: default;
  width: 100vw;
  height: 80vh;
}
body{
    background-color: rgb(15, 9, 43);
    
}
.ki-margin-large-top {
    margin-top: 150px;
}

.ki-black-text {
    color: #444444;
}
.ki-button {
    border: 1px solid #fff;
    color: #fff;
    background-color: rgba(0, 0, 0, 0);
    font-family: Montserrat-Regular;
}

#map{
    margin-left: auto;
    margin-right: auto;
}
.kol-flex-child{
    top: 0;
    left: 0;
    position: absolute;
    margin: 2%;
    display: flex;
    flex-direction: column;
    justify-content: flex-start;
    background-color: #fff;
   justify-content: center;
   position: relative; z-index:500;
   border-radius: 6px;
}
.kol-btn{
    /*background-color: #f6005c;
    color: #ffffff; */
    background-color: #1f1c3b;
    color: #bcbed0;
    font-family: Montserrat-Regular;
    margin: 25px 25px;
    padding: 8px 32px;
    border-radius: 6px; 
    border: 1px solid #bcbed0;
    display: inline-block;
    font-weight: bold;
    transition: background-color 0.3s ease-in;
    
}
.kol-flex {
    display: -webkit-box;
    display: -moz-box;
    display: -ms-flexbox;
    display: -webkit-flex;
    display: flex;
    
}
.kol-btn:hover {
    background-color: #423899;
    color: #ebedff;
    border: 1px solid #e2e5fa;
    }
.kol-btn:active{
    background-color: #251e6b;
    color: #ffff;
}
</style>