<template>
<div class="graphic" slot="graphic">
    <div v-if="currStep == '1' || currStep == '2'" id="three-intro">
        <div class="candles-back"></div>
        <div class="candle-group">
        <transition-group name="fade" tag="div" appear>
            <div v-if="currStep == '2'" key="a" id="candles" style="bottom: 65%; left: 56%">
                <img width="6%" src="~assets/candles.svg" alt="candles">   
            </div>
            <div v-if="currStep == '2'" key="b" id="candles" style="bottom: 63%; left: 53%">
                <img width="14%" src="~assets/candles.svg" alt="candles">   
            </div>
            <div v-if="currStep == '2'" key="c" id="candles" style="bottom: 60%; left: 53%">
                <img width="18%" src="~assets/candles.svg" alt="candles">   
            </div>
            <div v-if="currStep == '2'" key="d" id="candles" style="bottom: 58%; left: 50%">
                <img width="22%" src="~assets/candles.svg" alt="candles">   
            </div>
            <div v-if="currStep == '2'" key="e" id="candles" style="bottom: 53%; left: 44%">
                <img width="35%" src="~assets/candles.svg" alt="candles">   
            </div>
            <div v-if="currStep == '2'" key="f" id="candles" style="bottom: 40%; left: 30%">
                <img width="50%" src="~assets/candles.svg" alt="candles">   
            </div>
            <div v-if="currStep == '2'" key="g" id="candles" style="bottom: 30%; left: 20%">
                <img width="70%" src="~assets/candles.svg" alt="candles">   
            </div>
        </transition-group>
        <div id="candles" :style="{bottom: candlesBot + '%'}">
            <img width="100%" src="~assets/candles.svg" alt="candles">   
        </div>
        </div>
    </div>
    <div  v-if="currStep == '3'" id="vid-holder">
        <video id="citi-field-video" src="~assets/citi-field-short.mp4" muted="true" autoplay></video>
        <div class="vid-cover"></div>
        <!-- <div id="vid-text">That's enough to fill Citi Field baseball stadium in Queens, New York</div> -->
    </div>
    <div  v-show="currStep == 'stadium video'" id="vid-holder">
        <div class="br-dark"></div>
        <div class="rel">
            <video :style="{opacity: vOp}" id="google-earth-video" src="~assets/citi-googleearth-hd.mp4" muted="true" autoplay></video>
            <div :style="{opacity: vOp}" class="vid-cover" id="g-earth-cover"></div>
            <!-- <stadium :style="{opacity: stadiumOp, position: 'absolute', top:'-200px', left:'13vw', transform:'scale(0.55,0.55)'}"></stadium> -->
            <img v-show="subStep == 'flag'" id="flag-img" src="~static/flag.png"/>
            <seats :style="{opacity: stadiumOp, position: 'absolute', top:'275px', left:'575px', transform:'scale(1.38,1.38)'}"></seats>
            <!--  -->
        </div>
    </div>
    <div  v-if="currStep == 'rates'">
        <div class="br-dark"></div>
        <div class="seats-highlight-holder">
            <div v-show="subStep == 'seatsHighlight' || subStep == 'seatsHighlight2'" class="stadium-overlay-text2 c-grey" id="over-1">Gun deaths per 100,000 people</div>
            <seatHighlight v-show="subStep == 'seatsHighlight' || subStep == 'seatsHighlight2'" class="seats-highlight"></seatHighlight>
        </div>
        <img id="hundredk-img" src="~static/100k.png">
        <div v-show="subStep == 'rates2'" class="stadium-overlay-text c-white" id="over-1">100,000 people – 2.5&nbsp;Citi&nbsp;Field&nbsp;Stadiums</div>
        <!-- <div id="over-2 c-white">100,000</div> -->
    </div>
    <div v-if="currStep == 'age break' || currStep == 'age break highlight'">
        <div class="br-dark"></div>
        <div class="chart-holder">
            <line-chart id="age-break-1" :chart-data="cd.lineChartData" :options="lineChartOptionsLabels('Gun Death Rates by Age', 'Gun deaths per 100k residents', 'Age group')" :height="400" :width="950"></line-chart>
            <svg  v-if="currStep == 'age break highlight'" class="highlight" id="highlight-1" version="1.1" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" x="0px" y="0px"
                width="120.07px" height="120.07px" viewBox="0 0 120.07 120.07" style="enable-background:new 0 0 120.07 120.07;"
                xml:space="preserve">
            <defs>
            </defs>
            <circle class="st0" cx="60.04" cy="60.04" r="30"/>
            </svg>
            <svg  v-if="currStep == 'age break highlight'" class="highlight" id="highlight-2" version="1.1" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" x="0px" y="0px"
                width="120.07px" height="120.07px" viewBox="0 0 120.07 120.07" style="enable-background:new 0 0 120.07 120.07;"
                xml:space="preserve">
            <defs>
            </defs>
            <circle class="st0" cx="60.04" cy="60.04" r="30"/>
            </svg>
        </div>
    </div>
    <div v-if="currStep == 'age break 2' || currStep == 'age break 2 highlight'">
        <div class="br-dark"></div>
        <div class="chart-holder">
            <line-chart id="age-break-1" :chart-data="cd.lineChartData2" :options="lineChartOptionsLabels('Gun Death Rates by Age', 'Gun deaths per 100k residents', 'Age group')" :height="400" :width="950"></line-chart>
            <svg  v-if="currStep == 'age break 2 highlight'" class="highlight" id="highlight-3" version="1.1" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" x="0px" y="0px"
                width="120.07px" height="120.07px" viewBox="0 0 120.07 120.07" style="enable-background:new 0 0 120.07 120.07;"
                xml:space="preserve">
            <defs>
            </defs>
            <circle class="st0" cx="60.04" cy="60.04" r="30"/>
            </svg>
            <svg  v-if="currStep == 'age break 2 highlight'" class="highlight" id="highlight-4" version="1.1" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" x="0px" y="0px"
                width="120.07px" height="120.07px" viewBox="0 0 120.07 120.07" style="enable-background:new 0 0 120.07 120.07;"
                xml:space="preserve">
            <defs>
            </defs>
            <circle class="st0" cx="60.04" cy="60.04" r="30"/>
            </svg>
        </div>
    </div>
    <div v-if="currStep == 'even more' || currStep == 'peaks'">
        <div class="br-dark"></div>
        <div class="chart-holder">
            <div class="small-mult-holder">
                <div v-for="(race,index) in raceChartData" :key="index" class="race-chart-holder">
                        <line-chart ref="race-line-chart" :chart-data="raceChartData[index]" :options="lineChartPlainOptions" :height="100" :width="200"></line-chart>
                    <div class="race-label c-white">{{race['race-eth']}}</div>
                </div>
            </div>
        </div>
    </div>
    <div v-if="currStep == 'race age all'">
        <div class="br-dark"></div>
        <div class="line-chart-holder">
        <line-chart ref="race-line-chart" :chart-data="cd.ageRaceChartData" :options="lineChartPlainOptions" :height="400" :width="800"></line-chart>
        </div>
    </div>
    <div v-if="currStep == 'break down race'">
        <div class="br-dark"></div>
        <img id="bw-compare" src="~static/B_compare4.svg" alt="">
    </div>
    <div v-if="currStep == 'does not stop'">
        <div class="br-dark"></div>
    </div>
    <div v-if="currStep == 'us incarceration intro' || currStep == 'us incarceration' || currStep == 'homicide and incarceration'">
        <div class="br-dark"></div>
        <div class="section-title-overlay c-grey" v-if="currStep == 'us incarceration' || currStep == 'homicide and incarceration'">Incarcerated people per 100,000 residents</div>
        <incarCompare id="incarCompare"/>
    </div>
    <div v-if="currStep == 'firearm crimes'">
        <div class="br-dark"></div>
    </div>
    <div v-if="currStep == 'arrested'">
        <div class="br-dark"></div>
        <div class="line-chart-holder">
            <line-chart id="arrestChart" ref="race-line-chart" :chart-data="cd.arrestWeaponsLineChartData" :options="lineChartOptionsLabels('Arrest Rates for Weapons Possession by Race/Ethnicity', 'Arrests per 100k residents', 'Year')" :height="350" :width="700"></line-chart>
        </div>
    </div>
    <div v-if="currStep == 'jailed'">
        <div class="br-dark"></div>
        <div class="line-chart-holder">
            <line-chart ref="race-line-chart" :chart-data="cd.jailLineChartData" :options="lineChartOptionsLabels('Jail Rates by Race Ethnicity', 'Jailed people per 100k residents', 'Year')" :height="350" :width="700"></line-chart>
        </div>
    </div>
    <div v-if="currStep == 'alarmingly disparate'">
        <div class="br-dark"></div>
    </div>
    <div v-if="currStep == 'still disparate'">
        <div class="br-dark"></div>
        <div class="line-chart-holder">
            <line-chart ref="race-line-chart" :chart-data="cd.prisonerTimeLineChartData" :options="lineChartOptionsLabels('Incarceration Rates by Race/Ethnicity', 'Incarcerated people per 100k residents', 'Year')" :height="350" :width="700"></line-chart>
        </div>
    </div>
    <div v-if="currStep == 'criminality'">
        <div class="br-dark"></div>
        <img src="~static/grid_1.svg" alt="">
    </div>
    <div v-if="currStep == 'entire response'">
        <div class="br-dark"></div>
        <img src="~static/grid_1.2.svg" alt="">
    </div>
    <div v-if="currStep == 'perpetrators'">
        <div class="br-dark"></div>
        <img src="~static/grid_2.svg" alt="">
    </div>
    <div v-if="currStep == 'connected'">
        <div class="br-dark"></div>
        <img src="~static/grid_3.svg" alt="">
    </div>
    <div v-if="currStep == 'local strategies'">
        <div class="br-dark"></div>
        <img src="~static/grid_4.svg" alt="">
    </div>
    <div v-if="currStep == 'tailored support'">
        <div class="br-dark"></div>
        <img src="~static/focused_strategy.svg" alt="">
    </div>
    <div v-if="currStep == 'epidemic'">
        <div class="br-dark"></div>
        <img src="~static/tailored_support.svg" alt="">
    </div>
    <div v-if="currStep == 'diff solutions'">
        <div class="br-dark"></div>
        <img src="~static/local_types.svg" alt="">
    </div>
    <div v-if="currStep == 'common thread'">
        <div class="br-dark"></div>
        <img src="~static/common_thread.svg" alt="">
    </div>
    <div v-if="currStep == 'Richmond'">
        <div class="br-dark"></div>
        <div class="chart-holder">
            <line-chart id="richmond-chart" :chart-data="cd.richmondHomChartData" :options="lineChartOptionsLabels('Gun Homicides in Richmond, CA', 'Gun Homicides', 'Year')" :height="400" :width="950"></line-chart>
        </div>
    </div>
    <div v-if="currStep == 'Richmond2'">
        <div class="br-dark"></div>
        <div class="chart-holder">
            <line-chart id="richmond-chart2" :chart-data="cd.richmondCountChartData" :options="lineChartOptionsLabels('Non-fatal shootings in Richmond, CA', 'Shootings', 'Year')" :height="400" :width="950"></line-chart>
        </div>
    </div>
    <div v-if="currStep == 'media'">
        <div class="br-dark"></div>
        <img src="~static/media.svg" width="100%" alt="">
    </div>
    <div v-if="currStep == 'media2'">
        <div class="br-dark"></div>
        <img src="~static/media2.svg" width="100%" alt="">
    </div>
    <div v-if="currStep == 'mass police'">
        <div class="br-dark"></div>
        <img src="~static/mass_police.svg" width="100%" alt="">
    </div>
    <div v-if="currStep == 'meaningful solution'">
        <div class="br-dark"></div>
        <img src="~assets/candles.svg" width="100%" alt="">
    </div>
</div>
</template>

<script>

import stadium from '~/components/stadium.vue'
import seats from '~/components/stadium3.vue'
import LineChart from '~/components/LineChart'
import allChartDataJson from '~/static/allChartData.json'
import seatHighlight from '~/components/seat-highlight.vue'
import incarCompare from '~/components/incarc_compare.vue'
import allChartData from '~/static/one-chartdata.json'

export default {
    components: {
      LineChart,
      stadium,
      seats,
      seatHighlight,
      incarCompare,
    }
}
</script>