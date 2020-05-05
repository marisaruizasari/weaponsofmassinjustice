<template>
  <div>
    <!-- <navBar></navBar> -->
    <!-- <div id="three-intro" class="flex-row">
      <div class="section-title">Advancing Peace in Sacramento, CA</div>
      <img id="network-icon" width="300px" src="../assets/network.svg" alt="network">
    </div> -->
    <!-- <div id="three-intro" class="flex-row">
        <div class="head-img-wrap">
            <img id="header-img" src="~static/ap-img.png" alt="baseball field">
        </div>
        <div class="ap-head-title">Advancing Peace in Sacramento, CA</div>
    </div> -->
    <div class="flex-row merr">
      <div id="map-wrap">
        <div id="zoom-controls">
          <button @click="zoomMap" id="city" class="map-btn city">Sacramento City</button>
          <button @click="zoomMap" id="dph" class="map-btn dph">Del Paso Heights</button>
          <button @click="zoomMap" id="op" class="map-btn op">Oak Park</button>
          <button @click="zoomMap" id="ss" class="map-btn ss">South Sacramento</button>
        </div>
        <client-only>
          <l-map ref="myMap" :options="mapOptions" :zoom="zoom" :center="center">
            <l-tile-layer :url="url"></l-tile-layer>
            <l-geo-json class="city-boundaries" :geojson="test" :options="cbStyle"></l-geo-json>
            <l-geo-json claa="ap-zones" :geojson="ap" :options="apZoneStyle"></l-geo-json>
            <l-circle class="gv-data" :id="point.properties.UNIQUE_KEY" @mouseover="openPopup"  @mouseout="closePopup" v-for="point in filterData" :key="point.properties.UNIQUE_KEY" :fillColor="getColor(point)" :color="getColor(point)" :lat-lng="[point.geometry.coordinates[1], point.geometry.coordinates[0]]" :radius="circleStyle.radius">
              <l-popup class="popup">
                <p> Incident date: {{point.properties.OCC_DATE}} </p>
                <p> Location: {{point.properties.LOCATION.trim()}}</p>
                <p> Type: {{point.properties.OFFENSE_D.trim()}}</p>
              </l-popup>
            </l-circle>
          </l-map>
        </client-only>
      </div>
      <div id="sidebar">
        <div class="centered-row">
          <h3 class="section-title">Non-fatal & Fatal Shootings in Sacramento, CA</h3>
        </div>
        <div class="centered-row">
          <div class="filter-box">
            <p>Start date</p> <input id="start" v-model="start" type="date">
          </div>
          <div class="filter-box">
            <p>End date</p> <input id="end" v-model="end" type="date">
          </div>
          <div class="filter-box">
            <p>Type</p> 
            <select v-model="type">
              <option value="all">All shootings</option>
              <option value="non-fatal">Non-fatal shootings</option>
              <option value="fatal">Fatal shootings</option>
            </select>
          </div>
          
        </div>
        <div class="centered-col">
        <bar-chart id="shooting-bar" :chart-data="barChartData" :options="barChartOptions" :height="200"/>
        <line-chart id="shooting-line" :chart-data="lineChartData" :options="barChartOptions" :height="200"></line-chart>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import Logo from '~/components/Logo.vue'
import gvdata from '~/static/go-ap-final.json';
import cb from '~/static/city-boundaries.json';
import ap from '~/static/apzones.json';
import BarChart from '~/components/BarChart'
import LineChart from '~/components/LineChart'
import navBar from '~/components/navBar.vue'


const chartColors = {
  dph: '#EF915F',  //#51B07B
  op: '#4472F2',  //#8682FF
  ss: '#53B77E',  //#E86BAD
  city: '#797979' //#74806A
};

export default {
  data() { 
    return { 
      latlngs: [
        [50.5, 30.5, 0.2], // lat, lng, intensity
	      [50.6, 30.4, 0.5],
      ],
      datacollection: {
        },
      barChartOptions: {
        responsive: true,
        legend: {
          display: false,
        },
        title: {
          display: true,
          text: 'Shootings'
        },
        scales: {
          yAxes: [
            {
              ticks: {
                beginAtZero: true
              }
            }
          ]
        }
      },
      mapOptions: {
        zoomSnap: 0.25,
        loading: false, 
        show: true, 
        enableTooltip: true,
        zoomControl: false 
      }, 
      zoom: 11.25, 
      center: [38.56176, -121.46999],
      geojson: gvdata.features, 
      boundaries: null,
      apzones: null,
      zoneCenters: {
        city: {center: [38.56176, -121.46999], zoom: 11.5},
        dph: {center: [38.63644, -121.43971], zoom: 13},
        op: {center: [38.54618, -121.4587], zoom: 13.5},
        ss: {center: [38.47542, -121.45702], zoom: 12.75}
      },
      start: '2018-07-01',
      end: '2019-06-30',
      type: 'all',
      test: cb,
      ap: ap,
      pts: null,
      circleStyle: {
        color: 'red',
        fillColor: '#f03',
        fillOpacity: 0.5,
        radius: 30
      },
      apZoneStyle: {
        fillColor: '#9D9CA1',
        stroke: true,
        fill: false,
        weight: 4,
        color: 'white'
      }, 
      cbStyle: {
        fillColor: '#B3B2B8',
        // color: '#A8A6AB',
        stroke: false,
  
      },
      url: 'https://{s}.basemaps.cartocdn.com/light_all/{z}/{x}/{y}{r}.png',   //'https://api.mapbox.com/styles/v1/masari714/ck8z5ba1k02hv1intb0uv7jd8/tiles/256/{z}/{x}/{y}@2x?access_token=pk.eyJ1IjoibWFzYXJpNzE0IiwiYSI6ImNrOHo0OHdndTBlZm8zZnBvYTg1ZzRvenkifQ.aQpHyp0k8IHBvKr3qRSRaA',
      attribution: 'Map data &copy; <a href=&quot;https://www.openstreetmap.org/&quot;>OpenStreetMap</a> contributors, <a href=&quot;https://creativecommons.org/licenses/by-sa/2.0/&quot;>CC-BY-SA</a>, Imagery &copy; <a href=&quot;https://www.mapbox.com/&quot;>Mapbox</a>', 
      }; 
  },
  computed: {
    filterData() {
      return this.geojson.filter(x => {

        let filteredPoints = this.$moment(x.properties.OCC_DATE, 'MM/DD/YY') > this.$moment(this.start, 'YYYY-MM-DD') && this.$moment(x.properties.OCC_DATE, 'MM/DD/YY') < this.$moment(this.end, 'YYYY-MM-DD')
        
        if(this.type == "fatal") {
            filteredPoints = this.$moment(x.properties.OCC_DATE, 'MM/DD/YY') > this.$moment(this.start, 'YYYY-MM-DD') && this.$moment(x.properties.OCC_DATE, 'MM/DD/YY') < this.$moment(this.end, 'YYYY-MM-DD') && x.properties.OFFENSE_D.includes('187(A)')
        } else if (this.type == "non-fatal") {
            filteredPoints = this.$moment(x.properties.OCC_DATE, 'MM/DD/YY') > this.$moment(this.start, 'YYYY-MM-DD') && this.$moment(x.properties.OCC_DATE, 'MM/DD/YY') < this.$moment(this.end, 'YYYY-MM-DD') && x.properties.OFFENSE_D.includes('245')
        }

       return filteredPoints;
      })
    },
    barChartData() {

      let dphCount = this.filterData.filter(x => {
        return x.properties.PeaceZone == 'A'
      }).length
      let opCount = this.filterData.filter(x => {
        return x.properties.PeaceZone == 'B'
      }).length
      let ssCount = this.filterData.filter(x => {
        return x.properties.PeaceZone == 'C' || x.properties.PeaceZone == 'D'
      }).length
      let nonApCount = this.filterData.filter(x => {
        return x.properties.PeaceZone == null
      }).length


      console.log(dphCount)
      console.log(opCount)
      console.log(ssCount)
      console.log(nonApCount)


      let barChartData = {
        labels: ['Del Paso Heights', 'Oak Park', 'South Sac', 'Non-AP'],
        datasets: [
          {
            label: 'shootings',
            // backgroundColor: ["red", "orange", "yellow"],
            backgroundColor: [chartColors.dph, chartColors.op, chartColors.ss, chartColors.city],
            data: [dphCount, opCount, ssCount, nonApCount],
            barPercentage: 0.5
          }
        ]
      }

      return barChartData;
    },
    lineChartData() {

      var dateStart =  this.$moment(this.start, 'YYYY-MM-DD');
      var dateEnd =  this.$moment(this.end, 'YYYY-MM-DD');
      var monthValues = [];

      while (dateEnd > dateStart || dateStart.format('M') === dateEnd.format('M')) {
        monthValues.push(dateStart.format('MMM-YY'));
        dateStart.add(1,'month');
      }

      let dph = this.filterData.filter(x => {
        return x.properties.PeaceZone == 'A'
      })
      let op = this.filterData.filter(x => {
        return x.properties.PeaceZone == 'B'
      })
      let ss = this.filterData.filter(x => {
        return x.properties.PeaceZone == 'C' || x.properties.PeaceZone == 'D'
      })
      let nonAp = this.filterData.filter(x => {
        return x.properties.PeaceZone == null
      })

      let lineChartData = {
          labels: monthValues,
          datasets: [
            {
              label: 'Del Paso Heights',
              borderColor: chartColors.dph,
              fill: false,
              data: [2, 3, 6]
            }, 
            {
              label: 'Oak Park',
              borderColor: chartColors.op,
              fill: false,
              data: [1, 4, 5]
            },
            {
              label: 'South Sac',
              borderColor: chartColors.ss,
              fill: false,
              data: [4, 2, 7]
            },
            {
              label: 'Non-AP',
              borderColor: chartColors.city,
              fill: false,
              data: [4, 2, 7]
            },
          ]
        }

        this.aggregateByMonth(dph, lineChartData, 0)
        this.aggregateByMonth(op, lineChartData, 1)
        this.aggregateByMonth(ss, lineChartData, 2)
        this.aggregateByMonth(nonAp, lineChartData, 3)


      return lineChartData;
    }
  },
  methods: {
    aggregateByMonth(dataset, obj, index) {
      let o = {};
      // console.log(dataset)
     
      for (let x of dataset) {
        var dt_object = this.$moment(x.properties.OCC_DATE, 'MM/DD/YY'); // convert to datetime object
        var key =  dt_object.format('YY') + '-' + dt_object.format('MM');

      if (o[key] === undefined) {
          o[key] = [];
        };

      o[key].push(x)

      }

      var g = function(member_count){
          return member_count
      }

      let aggregated = [];
      for (let x in o) {
          let count = o[x].length
          o[x] = count
          aggregated.push(o[x])
      }

      console.log(aggregated)

      obj.datasets[index].data = aggregated
    },
    getColor(point) {
      let color;
      let zone = point.properties.PeaceZone;
      if (zone == "A") {
        color = chartColors.dph
      } else if (zone == "B") {
        color = chartColors.op
      } else if (zone == "C" || zone == "D") {
        color = chartColors.ss
      } else {
        color = chartColors.city
      }

      return color

    },
    zoomMap(e) {
      let zone = e.target.id.toString()
      console.log(e.target.id)
      console.log(this.zoneCenters[zone])
      this.$refs.myMap.mapObject.setView(this.zoneCenters[zone].center, this.zoneCenters[zone].zoom)
    },
    openPopup(e) {
      let circle = (e.target)
      let popup = e.target._popup._source
      popup._popup.options.closeButton = false
      e.target.openPopup(popup)
    },
    closePopup(e) {
      let circle = (e.target)
      let popup = e.target._popup._source
      e.target.closePopup(popup)
    },
    getData() {
      console.log('getting data')
      this.boundaries = cb.features;
      this.apzones = ap.features;
      this.pts = gvdata;
      console.log(cb)
      console.log(ap)
    },
    logInfo(point, event) {
      console.log(point.properties.OCC_DATE)
      console.log(event.target)
    }

  },
  components: {
    Logo,
    BarChart,
    LineChart,
    navBar
  },
  head() {
    return {
      script: [
        { src: 'https://identity.netlify.com/v1/netlify-identity-widget.js' }
      ]
    }
  }
}
</script>

<style scoped>

/* * {
 font-family: 'Merriweather', serif;
} */

.head-img-wrap {
    display: flex;
    flex-direction: row;
    justify-content: flex-end;
    overflow: hidden;
    height: 100vh;
    width: 100vw;
}
#header-img {
    height: 850px;
    width: auto;
}
.ap-head-title {
    font-family: 'Merriweather', serif;
    font-weight: bold;
    position: absolute;
    bottom: 300px;
    left: 380px;
    color: white;
    max-width: 400px;
    text-align: left;
    font-size: 40px;
    position: absolute;
}

#three-intro {
  justify-content: center;
  align-items: center;
  height: 100vh;
  z-index: 999999;
}

.nav-back {
  width: 100vw;
  top: 0;
  z-index: 99999999;
}
.filter-box {
  font-family: 'Merriweather', serif;
  margin: 0 5px;
}

.filter-box input, .filter-box option {
  font-size: 13px;
}
.section-title {
  font-family: 'Merriweather', serif;
  font-weight: 700;
  font-size: 22px;
  margin: 35px 0 30px 0;
  max-width: 65%;
  text-align: center;
}
.label {
  font-family: 'Merriweather', serif;
}
#shooting-bar, #shooting-line {
  width: 80%;
}
.map-btn {
  padding: 10px 15px;
  text-decoration: none;
  border-radius: 10px;
  border: .5px solid rgb(185, 185, 185);
  background-color: rgb(255, 255, 255);
  font-weight: bold;
  margin: 0 5px;
  font-size: 0.8em;
}

.dph {
  color: rgb(201, 134, 98);
}

.op {
 color: rgb(82, 118, 216); 
}

.ss {
 color: rgb(81, 156, 114);
}

.city {
 color: #3a3a3a;
}

#sidebar {
  width: 50vw;
  padding: 0 5vw;
  position: relative;
}
.centered-col {
  width: 100%;
  display: flex;
  flex-direction: column;
  align-items: center;
}

.centered-row {
  width: 100%;
  display: flex;
  flex-direction: row;
  flex-wrap: nowrap;
  justify-content: center;
}

.flex-row {
  display: flex;
  flex-direction: row;
  flex-wrap: nowrap;
  width: 100vw;
  height: 100vh;
}

.container {
  margin: 0 auto;
  min-height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  text-align: center;
}

#map-wrap {
  position: relative;
  height: 100vh; 
  width: 50vw
}

#zoom-controls {
  position: absolute;
  left: 10%;
  bottom: 25px;
  z-index: 10000;
}

.subtitle {
  font-weight: 300;
  font-size: 2rem;
  color: #526488;
  word-spacing: 5px;
  padding-bottom: 15px;
}
a {
  font-weight: bold;
  color: #526488;
}
.deploy-button {
  margin-top: 1rem;
}
</style>
