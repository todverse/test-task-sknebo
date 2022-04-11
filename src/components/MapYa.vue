/* eslint-disable */
<template> 
<div id='map'>
  <Notifications :arr="notifications" />
  <yandex-map 
    :settings="settings"
    :coords="coords"
    :zoom="9"
    @click="onClick"
    @map-was-initialized="init"
  >
  <ymap-marker 
      :coords="coords" 
      marker-id="1" 
      :hint-content="address" 
  />
  </yandex-map>
</div>
</template>

<script>
  import { yandexMap, ymapMarker } from 'vue-yandex-maps'
  import Notifications from './Notifications'

  export default {
    components : { yandexMap, ymapMarker, Notifications },

      data: () => ({
       coords: [
        55.753429006938546,
        37.63096318866614
      ],

      settings: {
        apiKey: '99a2f189-c861-4b8a-90e7-34fa2c7add0e',
      },

      address: 'Россия, Москва',
      instance: Object,
      Router: Object,
      lineGeoAir: Object,
      activeRouterDist: 'loading...',
      counter: 0,
      MCARmarks: [],
      notifications: [],
    }),

    methods: {
      onClick(e) {
      this.coords = e.get('coords');

      let closestMCAR = ymaps.geoQuery(this.MCARmarks).getClosestTo(this.coords);
      let airDist = ymaps.coordSystem.geo.getDistance(this.coords, closestMCAR.geometry.getCoordinates())/1000;

      if (this.counter === 0) { 
      this.Router = new ymaps.multiRouter.MultiRoute({   
        referencePoints: [
          e.get('coords'),
          closestMCAR,
        ]
        }, {
        boundsAutoApply: false
      });
      };
      this.counter++;

      this.Router.model.events.add('requestsuccess', () => {
        let activeRouter = this.Router.getActiveRoute();
        this.activeRouterDist = ' ' + activeRouter.properties.get('distance').text;
      });

      this.Router.model.setReferencePoints([
        this.coords,
        closestMCAR,
      ]);

      this.instance.geoObjects.remove(this.lineGeoAir);
      let lineAir = new ymaps.geometry.LineString([
        this.coords, closestMCAR.geometry.getCoordinates(),
      ]);
      this.lineGeoAir = new ymaps.GeoObject({ geometry: lineAir });
      this.instance.geoObjects.add(this.lineGeoAir);
      
      ymaps.geocode(e.get('coords')).then((res) => {
      this.instance.geoObjects.add(this.Router);
      this.address = res.geoObjects.get(0).properties.get('metaDataProperty').GeocoderMetaData.text;
      this.notifications.push({ 
        msg:`${this.address}`, 
        dist: `Расстояние по дороге ${this.activeRouterDist}`,
        distAir: `Расстояние по воздуху ${airDist.toFixed(2)} км`,
        })
      setTimeout(() => {
        this.notifications.shift();
      }, 5000);
      });

     },
     
    init(e) {
      this.instance = e;
      let marks = [
        [55.785017, 37.841576],
        [55.861979, 37.765992],
        [55.898533, 37.635961],
        [55.888897, 37.48861],
        [55.83251, 37.395275],
        [55.744789, 37.370248],
        [55.660424, 37.434424],
        [55.5922, 37.526366],
        [55.574019, 37.683167],
        [55.62913, 37.802473],
        [55.712203, 37.837121]
      ];

      marks.map((elem, index) => {
        this.MCARmarks[index] = new ymaps.Placemark(elem);
      });
    }
    },
  }
</script>

<style>
  #map {
    height: 800px;
  }
</style>
