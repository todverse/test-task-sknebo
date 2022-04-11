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
      marker-id="123" 
      :hint-content="hint" 
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
      hint: 'some hint',
      settings: {
        apiKey: '99a2f189-c861-4b8a-90e7-34fa2c7add0e',
      },
      address: String,
      instance: Object,
      Router: Object,
      counter: 0,
    }),
    methods: {
      onClick(e) {
      this.coords = e.get('coords');
      this.Router = new ymaps.multiRouter.MultiRoute({   
        referencePoints: [
          e.get('coords'),
          'МКАД ',
        ]
        }, {
        boundsAutoApply: true
      });
        this.Router.model.setReferencePoints([
          this.coords,
          'МКАД ',
        ]);
      ymaps.geocode(e.get('coords')).then((res) => {
      this.instance.geoObjects.add(this.Router);
      this.address = res.geoObjects.get(0).properties.get('metaDataProperty').GeocoderMetaData.text;
      document.getElementById('notification').style.display = 'block';
      setTimeout(() => {
        document.getElementById('notification').style.display = 'none';
      }, 1500);
      })
     },
    init(e) {
      this.instance = e;
    }
    },
  }
</script>

<style>
  #map {
    height: 500px;
  }
</style>