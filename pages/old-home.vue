<template>
  <div class="container">
    <div class="content">
    
      <client-only>
      <Scrollama
      :debug="false"
      :offset="0.5"
      @step-enter="changeGraphic"
    >
      <div class="graphic" slot="graphic">
        <Memorial></Memorial>
        <div class="fence-holder">
          <img src="~assets/gun.svg" alt="gun" id="gun">
        </div>
        <div class="fence-holder">
          <Polaroids id="polaroids"></Polaroids>
        </div>
      </div>
      <div class="step" data-step-no="1" sectionType="odd">
        <div class="col">
          <h2>Weapons <p class="of">of</p> Mass Injustice</h2>
          <p class="italic sub-title">Visualizing the unequal impacts of gun violence and the search for peace in America’s cities</p>
          <div class="line-sep-cont"><div class="line-sep"></div></div>
          <p>by Marisa Ruiz Asari</p>
        </div>
      </div>
      <div class="step" data-step-no="2" sectionType="even">
        <h2 id="select-story">Choose a story</h2>
      </div>
    </Scrollama>
    </client-only>
    <footer>
      About: This project was developed as partial fulfillment of the Parsons Masters in Data Visualization program.
    </footer>
    </div>
  </div>
</template>

<script>
import Polaroids from '~/components/Polaroids.vue'
import Memorial from '~/components/Memorial.vue'

export default {
  components: {
    Polaroids,
    Memorial
  },
  data() {
    return {
      currStep: null
    };
  },
  methods: {
    changeGraphic(event) {
      console.log(event)
      console.log(event.element.getAttribute('sectionType'))
      let step = event.element.getAttribute('data-step-no')
      let gun = document.getElementById('gun')
      let selectStory = document.querySelector('#polaroids')
      
      if (step == "1") {
        gun.style.visibility = 'visible';
        selectStory.style.visibility = 'hidden';
      }

      if (step == "2") {
        gun.style.visibility = 'hidden';
        selectStory.style.visibility = 'visible';
      }
    }
  },
  watch: {
    currStep: {
      deep: true,
      handler (change, old) {
      console.log('hi felix', change, old)
    }
    }
  }
}
</script>


<style lang="scss" scoped>


.greyedSVG {
  fill: #E4E4E4;
}

.heart {
  display: none;
  width: 200px; 
}

.container {
  margin: 0 auto;
  min-height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  text-align: center;
}

.content {
  width: 100vw;
}


.subtitle {
  font-weight: 300;
  font-size: 42px;
  color: #526488;
  word-spacing: 5px;
  padding-bottom: 15px;
}

.links {
  padding-top: 15px;
}
#app {
  font-family: 'Merriweather', serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
.intro,
.outro {
  padding: 20vh;
}


.scrollama-container {
  display: flex;
  .scrollama-graphic {
    position: sticky;
    flex: 5;
    height: 100vh;
    top: 0;
  }
  .scrollama-steps {
    flex: 3.5;
  }
}

.resize-observer[data-v-b329ee4c] {
    position: absolute;
    top: 0;
    left: 0;
    z-index: -1;
    width: 100%;
    height: 100%;
    border: none;
    background-color: transparent;
    pointer-events: none;
    display: block;
    overflow: hidden;
    opacity: 0;
}

// your elements styles
.graphic {
  height: 100%;
  position: relative;
  // margin: 0 3rem;
  // border: 1px solid #ccc;
  // background-color: #eee;
  font-size: 10rem;
  display: flex;
  align-items: center;
  justify-content: center;
}
.step {
  height: 100vh;
  position: relative;
  padding: 20vh 0;
  margin: 0 3rem;
  margin-bottom: 10vh;
  // background-color: beige;
  // border: 1px solid #ccc;
  display: flex;
  align-items: center;
  justify-content: center;
  &:last-child {
    margin-bottom: 0;
  }
}

.col {
  padding: 0 8vw;
}

#memorial {
  width: 100%;
}

.fence-holder {
   position: absolute;
   top: 10%;
   width: 80%;
   display: flex;
   flex-direction: row;
   flex-wrap: nowrap;
   justify-content: center;
}

#gun {
  width: 35%;
  padding-top: 50px;
}

#select-story {
  font-size: 35px;
}

.line-sep-cont {
  margin: 40px 0;
  display: flex;
  flex-direction: row;
  flex-wrap: nowrap;
  justify-content: center;
}

.line-sep {
  width: 30px;
  height: 2px;
  background: red;
}

h2, p {
  font-family: 'Merriweather', serif;
}

h2 {
  margin: 20px 0;
  font-size: 2.5rem;
}

p {
  margin: 10px 0;
}

.of {
  font-size: 20px;
  font-style: italic;
}

.sub-title {
  font-size: 18px;
  margin-top: 50px;
}

.italic {
  font-style: italic;
}

footer {
  font-family: 'Merriweather', serif;
  font-size: 15px;
  color: grey;
  height: 350px;
  display: flex;
  flex-direction: row;
  justify-content: center;
  align-items: center;
  padding-bottom: 30px;
  background: rgb(243, 243, 243);
}

</style>
