<template> 
<div id='map'>
  <Notifications :msg='address' />
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
      counter: 0,
      MCARmarks: [],
    }),

    methods: {
      onClick(e) {
      this.coords = e.get('coords');

      let closestMCAR = ymaps.geoQuery(this.MCARmarks).getClosestTo(this.coords);
      
      if(this.counter === 0) { 
      this.Router = new ymaps.multiRouter.MultiRoute({   
        referencePoints: [
          e.get('coords'),
          closestMCAR,
        ]
        }, {
        boundsAutoApply: false
      });
      this.counter++;
      };

      this.Router.model.setReferencePoints([
        this.coords,
        closestMCAR,
      ]);

      this.Router.model.events.add('requestsuccess', () => {
        let activeRouter = this.Router.getActiveRoute();
        console.log(activeRouter.getPaths().properties.get('distance'));
      });

      ymaps.geocode(e.get('coords')).then((res) => {
      this.instance.geoObjects.add(this.Router);
      this.address = res.geoObjects.get(0).properties.get('metaDataProperty').GeocoderMetaData.text;
      document.getElementById('notification').style.display = 'block';
      setTimeout(() => {
        document.getElementById('notification').style.display = 'none';
      }, 1500);
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
    height: 500px;
  }
</style>