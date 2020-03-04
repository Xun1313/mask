<template>
  <div id="app">
    <l-map style="width: 100%; height: 600px;" :zoom="zoom" :center="center" v-if="features.length > 0">
      <l-tile-layer :url="url" :attribution="attribution"></l-tile-layer>
      <v-marker-cluster>
        <l-marker :lat-lng="[item.geometry.coordinates[1], item.geometry.coordinates[0]]" v-for="item in features" :key="item.properties.id">
          <!-- :lat-lng="L.latLng(item.geometry.coordinates[1], item.geometry.coordinates[0])" -->
          <l-icon
            :icon-size="[100, 100]"
            :icon-anchor="[38, 20]"
            :icon-url="
              require(`./assets/source/Icon_location_${item.properties.mask_adult + item.properties.mask_child > 50 ? (item.properties.mask_adult + item.properties.mask_child > 100 ? 'green' : 'orange') : 'red'}.svg`)
            "
          >
          </l-icon>
          <l-popup
            :content="
              `<article class=&quot;info&quot;>
      <h4 class=&quot;info-title&quot;>${item.properties.name}</h4>
      <p class=&quot;info-content&quot;>${item.properties.address}</p>
      <p class=&quot;info-content&quot;>營業時間 | ${item.properties.service_periods}</p>
      <p class=&quot;info-content&quot;>連絡電話 | ${item.properties.phone}</p>
      <p class=&quot;info-new&quot;>資訊更新於 ${item.properties.updated}</p>
      <div class=&quot;info-group&quot;>
        <p class=&quot;info-group-amount info-group-adult&quot;>成人口罩 ${item.properties.mask_adult}個</p>
        <p class=&quot;info-group-amount info-group-kid&quot;>兒童口罩 ${item.properties.mask_child}個</p>
      </div>
      <a href=&quot;/&quot; class=&quot;info-route&quot;>Google 路線導航</a>
    </article>`
            "
          ></l-popup>
        </l-marker>
      </v-marker-cluster>
    </l-map>
    <!-- <article class="info">
      <h4 class="info-title"></h4>
      <p class="info-content"></p>
      <p class="info-content"></p>
      <p class="info-content"></p>
      <p class="info-new"></p>
      <div class="info-group">
        <p class="info-group-amount info-group-adult"></p>
        <p class="info-group-amount info-group-kid"></p>
      </div>
      <router-link to="/" class="info-route"></router-link>
    </article> -->
  </div>
</template>

<script>
import L from 'leaflet'
import { LMap, LTileLayer, LMarker, LPopup, LIcon } from 'vue2-leaflet'
/* import * as Vue2Leaflet from 'vue2-leaflet' */
import 'leaflet/dist/leaflet.css'
import Vue2LeafletMarkerCluster from 'vue2-leaflet-markercluster'
import { Icon } from 'leaflet'
/* delete Icon.Default.prototype._getIconUrl
Icon.Default.mergeOptions({
  iconRetinaUrl: require('leaflet/dist/images/marker-icon-2x.png'),
  iconUrl: require('leaflet/dist/images/marker-icon.png'),
  shadowUrl: require('leaflet/dist/images/marker-shadow.png')
}) */
export default {
  name: 'App',
  data() {
    return {
      test: [
        {
          type: 'Feature',
          properties: {
            id: '5901012203',
            name: '博荃藥局',
            phone: '02 -27316736',
            address: '台北市松山區敦化北路4巷51號',
            mask_adult: 6,
            mask_child: 60,
            updated: '2020\/02\/11 16:23:38',
            available:
              '星期一上午看診、星期二上午看診、星期三上午看診、星期四上午看診、星期五上午看診、星期六上午看診、星期日上午休診、星期一下午看診、星期二下午看診、星期三下午看診、星期四下午看診、星期五下午看診、星期六下午看診、星期日下午休診、星期一晚上看診、星期二晚上看診、星期三晚上看診、星期四晚上看診、星期五晚上看診、星期六晚上看診、星期日晚上休診',
            note: '如遇國定連續假期,藥局公休',
            custom_note: '',
            website: '',
            county: '臺北市',
            town: '松山區',
            cunli: '中正里',
            service_periods: 'NNNNNNYNNNNNNYNNNNNNY',
            service_note: '如遇國定連續假期,藥局公休'
          },
          geometry: {
            type: 'Point',
            coordinates: [121.544742, 25.050063]
          }
        },
        {
          type: 'Feature',
          properties: {
            id: '5901012383',
            name: '德川中西藥局',
            phone: '02 -27683399',
            address: '台北市松山區八德路4段96號',
            mask_adult: 0,
            mask_child: 120,
            updated: '2020\/02\/11 16:23:38',
            available:
              '星期一上午看診、星期二上午看診、星期三上午看診、星期四上午看診、星期五上午看診、星期六上午休診、星期日上午休診、星期一下午看診、星期二下午看診、星期三下午看診、星期四下午看診、星期五下午看診、星期六下午休診、星期日下午看診、星期一晚上看診、星期二晚上看診、星期三晚上看診、星期四晚上看診、星期五晚上看診、星期六晚上休診、星期日晚上休診',
            note: '',
            custom_note: '',
            website: '',
            county: '臺北市',
            town: '松山區',
            cunli: '復盛里',
            service_periods: 'NNNNNYYNNNNNYNNNNNNYY',
            service_note: ''
          },
          geometry: {
            type: 'Point',
            coordinates: [121.56066, 25.04836]
          }
        },
        {
          type: 'Feature',
          properties: {
            id: '5901012409',
            name: '家音藥局',
            phone: '02 -37652080',
            address: '台北市松山區民生東路5段73號',
            mask_adult: 24,
            mask_child: 30,
            updated: '2020\/02\/11 16:23:38',
            available:
              '星期一上午看診、星期二上午看診、星期三上午看診、星期四上午看診、星期五上午看診、星期六上午看診、星期日上午看診、星期一下午看診、星期二下午看診、星期三下午看診、星期四下午看診、星期五下午看診、星期六下午看診、星期日下午看診、星期一晚上看診、星期二晚上看診、星期三晚上看診、星期四晚上看診、星期五晚上看診、星期六晚上看診、星期日晚上看診',
            note: '早上9:00到晚上11:00',
            custom_note: '',
            website: '',
            county: '臺北市',
            town: '松山區',
            cunli: '東榮里',
            service_periods: 'NNNNNNNNNNNNNNNNNNNNN',
            service_note: '早上9:00到晚上11:00'
          },
          geometry: {
            type: 'Point',
            coordinates: [121.558489, 25.058709]
          }
        },
        {
          type: 'Feature',
          properties: {
            id: '5901012454',
            name: '敦北藥局',
            phone: '02 -27155691',
            address: '台北市松山區民生東路4段80巷5號',
            mask_adult: 0,
            mask_child: 78,
            updated: '2020\/02\/11 16:23:38',
            available:
              '星期一上午看診、星期二上午看診、星期三上午看診、星期四上午看診、星期五上午看診、星期六上午看診、星期日上午看診、星期一下午看診、星期二下午看診、星期三下午看診、星期四下午看診、星期五下午看診、星期六下午看診、星期日下午看診、星期一晚上看診、星期二晚上看診、星期三晚上看診、星期四晚上看診、星期五晚上看診、星期六晚上看診、星期日晚上看診',
            note: '',
            custom_note: '',
            website: '',
            county: '臺北市',
            town: '松山區',
            cunli: '東昌里',
            service_periods: 'NNNNNNNNNNNNNNNNNNNNN',
            service_note: ''
          },
          geometry: {
            type: 'Point',
            coordinates: [121.553125, 25.057129]
          }
        },
        {
          type: 'Feature',
          properties: {
            id: '5901012490',
            name: '中美藥局',
            phone: '02 -27627468',
            address: '台北市松山區富錦街531號',
            mask_adult: 0,
            mask_child: 51,
            updated: '2020\/02\/11 16:23:38',
            available:
              '星期一上午看診、星期二上午看診、星期三上午看診、星期四上午看診、星期五上午看診、星期六上午看診、星期日上午看診、星期一下午看診、星期二下午看診、星期三下午看診、星期四下午看診、星期五下午看診、星期六下午看診、星期日下午看診、星期一晚上看診、星期二晚上看診、星期三晚上看診、星期四晚上看診、星期五晚上看診、星期六晚上看診、星期日晚上看診',
            note: '營業時間如有異動,以藥局公告為準',
            custom_note: '',
            website: '',
            county: '臺北市',
            town: '松山區',
            cunli: '新益里',
            service_periods: 'NNNNNNNNNNNNNNNNNNNNN',
            service_note: '營業時間如有異動,以藥局公告為準'
          },
          geometry: {
            type: 'Point',
            coordinates: [121.565481, 25.061285]
          }
        },
        {
          type: 'Feature',
          properties: {
            id: '5901012507',
            name: '康麗藥局',
            phone: '02 -27174117',
            address: '台北市松山區南京東路3段303巷20號',
            mask_adult: 4,
            mask_child: 30,
            updated: '2020\/02\/11 16:23:38',
            available:
              '星期一上午看診、星期二上午看診、星期三上午看診、星期四上午看診、星期五上午看診、星期六上午看診、星期日上午休診、星期一下午看診、星期二下午看診、星期三下午看診、星期四下午看診、星期五下午看診、星期六下午看診、星期日下午休診、星期一晚上看診、星期二晚上看診、星期三晚上看診、星期四晚上看診、星期五晚上看診、星期六晚上看診、星期日晚上休診',
            note: '春節營業時間請電洽',
            custom_note: '',
            website: '',
            county: '臺北市',
            town: '松山區',
            cunli: '中正里',
            service_periods: 'NNNNNNYNNNNNNYNNNNNNY',
            service_note: '春節營業時間請電洽'
          },
          geometry: {
            type: 'Point',
            coordinates: [121.546826, 25.05369]
          }
        },
        {
          type: 'Feature',
          properties: {
            id: '5901012516',
            name: '惠登藥局',
            phone: '02 -25472432',
            address: '台北市松山區民生東路3段130巷34號',
            mask_adult: 0,
            mask_child: 62,
            updated: '2020\/02\/11 16:23:38',
            available:
              '星期一上午看診、星期二上午看診、星期三上午看診、星期四上午看診、星期五上午看診、星期六上午看診、星期日上午看診、星期一下午看診、星期二下午看診、星期三下午看診、星期四下午看診、星期五下午看診、星期六下午看診、星期日下午看診、星期一晚上看診、星期二晚上看診、星期三晚上看診、星期四晚上看診、星期五晚上看診、星期六晚上看診、星期日晚上看診',
            note: '',
            custom_note: '',
            website: '',
            county: '臺北市',
            town: '松山區',
            cunli: '松基里',
            service_periods: 'NNNNNNYNNNNNNYNNNNNNY',
            service_note: ''
          },
          geometry: {
            type: 'Point',
            coordinates: [121.546113, 25.056198]
          }
        },
        {
          type: 'Feature',
          properties: {
            id: '5901012561',
            name: '松田藥局',
            phone: '02 -25798005',
            address: '台北市松山區延吉街9－6號',
            mask_adult: 0,
            mask_child: 72,
            updated: '2020\/02\/11 16:23:38',
            available:
              '星期一上午看診、星期二上午看診、星期三上午看診、星期四上午看診、星期五上午看診、星期六上午看診、星期日上午休診、星期一下午看診、星期二下午看診、星期三下午看診、星期四下午看診、星期五下午看診、星期六下午看診、星期日下午休診、星期一晚上看診、星期二晚上看診、星期三晚上看診、星期四晚上看診、星期五晚上看診、星期六晚上看診、星期日晚上休診',
            note: '',
            custom_note: '',
            website: '',
            county: '臺北市',
            town: '松山區',
            cunli: '吉仁里',
            service_periods: 'NNNNNNYNNNNNNYNNNNNNY',
            service_note: ''
          },
          geometry: {
            type: 'Point',
            coordinates: [121.55355, 25.047007]
          }
        },
        {
          type: 'Feature',
          properties: {
            id: '5901012570',
            name: '予志藥局',
            phone: '02 -27623522',
            address: '台北市松山區富錦街521號',
            mask_adult: 0,
            mask_child: 36,
            updated: '2020\/02\/11 16:23:38',
            available:
              '星期一上午看診、星期二上午看診、星期三上午看診、星期四上午看診、星期五上午看診、星期六上午看診、星期日上午看診、星期一下午看診、星期二下午看診、星期三下午看診、星期四下午看診、星期五下午看診、星期六下午看診、星期日下午看診、星期一晚上看診、星期二晚上看診、星期三晚上看診、星期四晚上看診、星期五晚上看診、星期六晚上看診、星期日晚上休診',
            note: '',
            custom_note: '',
            website: '',
            county: '臺北市',
            town: '松山區',
            cunli: '富錦里',
            service_periods: 'NNNNNNNNNNNNNNNNNNNNY',
            service_note: ''
          },
          geometry: {
            type: 'Point',
            coordinates: [121.564944, 25.061186]
          }
        },
        {
          type: 'Feature',
          properties: {
            id: '5901012598',
            name: '婕登藥局',
            phone: '02 -27482893',
            address: '台北市松山區八德路4段699號',
            mask_adult: 6,
            mask_child: 31,
            updated: '2020\/02\/11 16:23:38',
            available:
              '星期一上午看診、星期二上午看診、星期三上午看診、星期四上午看診、星期五上午看診、星期六上午看診、星期日上午休診、星期一下午看診、星期二下午看診、星期三下午看診、星期四下午看診、星期五下午看診、星期六下午看診、星期日下午休診、星期一晚上看診、星期二晚上看診、星期三晚上看診、星期四晚上看診、星期五晚上看診、星期六晚上看診、星期日晚上看診',
            note: '',
            custom_note: '',
            website: '',
            county: '臺北市',
            town: '松山區',
            cunli: '慈祐里',
            service_periods: 'NNNNNNYNNNNNNYNNNNNNN',
            service_note: ''
          },
          geometry: {
            type: 'Point',
            coordinates: [121.576644, 25.050207]
          }
        },
        {
          type: 'Feature',
          properties: {
            id: '5901012632',
            name: '晨安藥局',
            phone: '02 -25798460',
            address: '台北市松山區八德路3段155巷20弄2號',
            mask_adult: 0,
            mask_child: 75,
            updated: '2020\/02\/11 16:23:38',
            available:
              '星期一上午看診、星期二上午看診、星期三上午看診、星期四上午看診、星期五上午看診、星期六上午休診、星期日上午休診、星期一下午看診、星期二下午看診、星期三下午看診、星期四下午看診、星期五下午看診、星期六下午看診、星期日下午看診、星期一晚上看診、星期二晚上看診、星期三晚上看診、星期四晚上看診、星期五晚上看診、星期六晚上看診、星期日晚上看診',
            note: '',
            custom_note: '',
            website: '',
            county: '臺北市',
            town: '松山區',
            cunli: '復勢里',
            service_periods: 'NNNNNYYNNNNNNNNNNNNNN',
            service_note: ''
          },
          geometry: {
            type: 'Point',
            coordinates: [121.555326, 25.049137]
          }
        },
        {
          type: 'Feature',
          properties: {
            id: '5901012650',
            name: '銘德藥局',
            phone: '02 -27511833',
            address: '台北市松山區八德路2段366巷28號',
            mask_adult: 2,
            mask_child: 78,
            updated: '2020\/02\/11 16:23:38',
            available:
              '星期一上午看診、星期二上午看診、星期三上午看診、星期四上午看診、星期五上午看診、星期六上午看診、星期日上午看診、星期一下午看診、星期二下午看診、星期三下午看診、星期四下午看診、星期五下午看診、星期六下午看診、星期日下午看診、星期一晚上看診、星期二晚上看診、星期三晚上看診、星期四晚上看診、星期五晚上看診、星期六晚上看診、星期日晚上看診',
            note: '',
            custom_note: '',
            website: '',
            county: '臺北市',
            town: '松山區',
            cunli: '中崙里',
            service_periods: 'NNNNNNNNNNNNNNNNNNNNN',
            service_note: ''
          },
          geometry: {
            type: 'Point',
            coordinates: [121.545925, 25.047242]
          }
        },
        {
          type: 'Feature',
          properties: {
            id: '5901012669',
            name: '政昇藥局',
            phone: '02 -27560286',
            address: '台北市松山區南京東路5段356號13樓',
            mask_adult: 0,
            mask_child: 148,
            updated: '2020\/02\/11 16:23:38',
            available:
              '星期一上午看診、星期二上午看診、星期三上午看診、星期四上午看診、星期五上午看診、星期六上午休診、星期日上午休診、星期一下午看診、星期二下午看診、星期三下午看診、星期四下午看診、星期五下午看診、星期六下午休診、星期日下午休診、星期一晚上看診、星期二晚上看診、星期三晚上看診、星期四晚上看診、星期五晚上看診、星期六晚上休診、星期日晚上休診',
            note: '',
            custom_note: '',
            website: '',
            county: '臺北市',
            town: '松山區',
            cunli: '新聚里',
            service_periods: 'NNNNNYYNNNNNYYNNNNNYY',
            service_note: ''
          },
          geometry: {
            type: 'Point',
            coordinates: [121.569691, 25.051063]
          }
        },
        {
          type: 'Feature',
          properties: {
            id: '5901012712',
            name: '立康大藥局',
            phone: '02 -27471749',
            address: '台北市松山區八德路四段2－1號',
            mask_adult: 0,
            mask_child: 18,
            updated: '2020\/02\/11 16:23:38',
            available:
              '星期一上午看診、星期二上午看診、星期三上午看診、星期四上午看診、星期五上午看診、星期六上午看診、星期日上午看診、星期一下午看診、星期二下午看診、星期三下午看診、星期四下午看診、星期五下午看診、星期六下午看診、星期日下午看診、星期一晚上看診、星期二晚上看診、星期三晚上看診、星期四晚上看診、星期五晚上看診、星期六晚上看診、星期日晚上看診',
            note: '',
            custom_note: '',
            website: '',
            county: '臺北市',
            town: '松山區',
            cunli: '復盛里',
            service_periods: 'NNNNNNNNNNNNNNNNNNNNN',
            service_note: ''
          },
          geometry: {
            type: 'Point',
            coordinates: [121.558111, 25.048117]
          }
        }
      ],
      icon: L.icon({
        iconUrl: require('./assets/source/Icon_location_orange.svg'),
        iconSize: [100, 100],
        iconAnchor: [16, 37]
      }),
      L,
      zoom: 8,
      center: [25.033976, 121.5623502],
      url: 'http://{s}.tile.osm.org/{z}/{x}/{y}.png',
      attribution: '&copy; <a href="http://osm.org/copyright">OpenStreetMap</a> contributors',
      features: []
    }
  },
  mounted() {
    /* if (navigator.geolocation) {
      navigator.geolocation.getCurrentPosition(position => {
        this.center = L.latLng(position.coords.latitude, position.coords.longitude)
      })
    } */
    this.$http.get('https://raw.githubusercontent.com/kiang/pharmacies/master/json/points.json').then(res => {
      console.log(res.data.features)
      this.features = res.data.features
    })
  },
  components: {
    LMap,
    LTileLayer,
    LMarker,
    LPopup,
    LIcon,
    'v-marker-cluster': Vue2LeafletMarkerCluster
  }
}
</script>

<style lang="scss">
@import './assets/_variables.scss';
@import '~leaflet.markercluster/dist/MarkerCluster.css';
@import '~leaflet.markercluster/dist/MarkerCluster.Default.css';
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
.info {
  &-title {
    font-size: $title;
    margin-bottom: 10px;
  }
  &-content {
    font-size: $content;
    margin: 0 0 5px 0 !important;
  }
  &-new {
    opacity: 0.7;
    margin: 0 0 5px 0 !important;
  }
  &-group {
    display: flex;
    align-items: center;
    margin: 0 0 10px 0 !important;
    &-amount {
      border-radius: 10px;
      font-size: $content;
      padding: 10px 0;
      text-align: center;
      width: 100%;
      margin: 0 !important;
    }
    &-adult {
      color: #ff973b;
      background-color: #ff973b29;
    }
    &-kid {
      margin-left: 5px !important;
      color: #f8be00;
      background-color: #ffdb4d33;
    }
  }
  &-route {
    text-decoration: none;
    display: block;
    color: white !important;
    background-color: #68af87;
    border-radius: 10px;
    font-size: $content;
    padding: 10px 0;
    text-align: center;
    transition: 0.5s all;
    &:hover {
      background-color: darken(#68af87, 10%);
    }
  }
}
</style>
