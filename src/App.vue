<template>
  <div class="tpc">
    <header>
      <div class="card">
        <div class="front">
          <h1>Terraria Progress</h1>
        </div>
        <div class="back" v-bind:class="{ 'tpc__info--active': progressionPercentage !== 0 }">
          <div class="progress">
            <img src="./assets/stopwatch.png" />
            <el-progress type="circle"
              :width="65"
              :percentage="progressionPercentage"
              :show-text="false"
              :status="progressionPercentage === 100 ? 'success' : ''">
              </el-progress>
          </div>
          <div class="tipsSwitch">
            <img src="./assets/announcement_box.png" />
            <el-switch
              v-model="showDesc"
              on-text=""
              off-text="">
            </el-switch>
          </div>
          <div class="clearProgression">
            <img src="./assets/trash_can.png" @click="clearProgression"  />
            <b> {{ progressionTotal  }} / {{ progressionData.length }} </b>
          </div>
        </div>
      </div>
    </header>
    <main>
      <ul>
        <li
          v-for="(item, index) in progressionData"
          :key="item.key">
          <el-checkbox
            @change="onToggle(item.key)"
            v-model="progression[item.key]">
            <span>
              <b>{{ item.label }}<tip v-if="showTips && (item.tips && item.tips.length)" :tips="item.tips"></tip></b>
              <small v-if="showDesc"> - {{ item.desc }}</small>
            </span>
          </el-checkbox>
        </li>
      </ul>
    </main>
  </div>
</template>
<script>
import PROGRESSION from '@/progression.json'
import localforage from 'localforage'
import Tip from '@/components/Tip.vue'

export default {
  name: 'app',
  components: {
    Tip
  },
  data () {
    const progression = PROGRESSION.reduce((acc, cur) => {
      acc[cur.key] = false
      return acc
    }, {})

    return {
      progression,
      progressionTotal: 0,
      progressionData: PROGRESSION,
      showDesc: true,
      showTips: true
    }
  },
  computed: {
    'progressionPercentage' () {
      return Math.round((this.progressionTotal / this.progressionData.length) * 100)
    }
  },
  mounted () {
    localforage.getItem('progression').then(localProgression => {
      let menudelay = 1500
      if (localProgression) {
        this.progression = localProgression
        this.progressionTotal = Object.keys(localProgression).reduce((total, key) => {
          total += localProgression[key] ? 1 : 0
          return total
        }, 0)
        menudelay = 0
      }
      setTimeout(() => {
        document.querySelector('.card').classList.add('card--flipped')
      }, menudelay)
    }).catch(err => {
      console.log(err)
    })
  },
  methods: {
    clearProgression () {
      this.progressionTotal = 0
      this.progression = PROGRESSION.reduce((acc, cur) => {
        acc[cur.key] = false
        return acc
      }, {})
    },
    onToggle (key) {
      this.progressionTotal += this.progression[key] ? 1 : -1
      localforage.setItem('progression', this.progression)
    }
  }
}
</script>
<style>
* {
  box-sizing: border-box;
  -webkit-touch-callout: none;
  -webkit-user-select: none;
  -khtml-user-select: none;
  -moz-user-select: none;
  -ms-user-select: none;
  user-select: none;
  -webkit-tap-highlight-color: rgba(255, 255, 255, 0);
}
html, body {
  height: 100%;
  margin: 0;
  padding: 0;
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
}
.tpc {
  height: 100%;
  padding-top: 124px;
}
.tpc > header {
  -webkit-perspective: 800;
  perspective: 800;
  height: 100px;
  width: 100%;
  margin: 0 0 1.5rem;
  position: fixed;
  top: 0;
  z-index: 2;
}
.card.card--flipped {
  -webkit-transform: rotateX(-180deg);
  transform: rotateX(-180deg);
}
.card {
  height: 100%;
  width: 100%;
  -webkit-transform-style: preserve-3d;
  transform-style: preserve-3d;
  -webkit-transition: 0.8s ease-out;
  transition: 0.8s ease-out;
  transform-origin: center;
}

.card .front,
.card .back {
  width: 100%;
  height: 100%;
  position: absolute;
  background: #fff;
  -webkit-backface-visibility: hidden;
  backface-visibility: hidden;
  line-height: 100px;
  z-index: 2;
  box-shadow: 0 1px 3px rgba(0,0,0,0.12), 0 1px 2px rgba(0,0,0,0.24);
}

.card .front {
  z-index: 1;
  text-align: center;
}

.card .front h1 {
  font-size: 1.5rem;
  line-height: 100px;
  margin: 0;
}

.card .back {
  -webkit-transform: rotatex(-180deg);
  transform: rotatex(-180deg);
  padding: 0;
  margin: 0;
  background: #fff;
  display: flex;
  flex-flow: row wrap;
  align-items: center;
  justify-content: space-around;
}

.progress {
  position: relative;
  display: flex;
  flex-flow: row wrap;
  align-items: center;
  justify-content: center;
  flex: 1;
}
.progress img {
  position: absolute;
}

.tipsSwitch,
.clearProgression {
  flex: 1;
  text-align: center;
  display: flex;
  flex-flow: column wrap;
  justify-content: space-between;
  height: 65px;
}
.tipsSwitch img,
.clearProgression img {
  flex: 0 32px;
  width: 32px;
  margin: 0 auto;
}
.clearProgression b {
  line-height: 22px;
}

main {
  width: 100%;
  display: flex;
  flex-flow: row wrap;
  padding: 0 1.5rem 1.5rem;
}

main ul,
main li {
  margin: 0;
  padding: 0;
  list-style: none;
}

main ul {
  display: flex;
  flex-flow: row wrap;
  width: 100%;
}

main li {
  flex: 0 100%;
  margin-bottom: 0.75rem;
}

main span {
  transition: opacity 0.6s ease;
  white-space: normal;
}
.is-checked+span {
  font-style: italic;
  color: rgb(19, 206, 102);
  text-decoration: line-through;
  opacity: 0.5;
}
.is-checked+span > span { color: #2c3e50; }

main > label {
  flex: 0 100%;
  margin-left: 0 !important;
  white-space: normal;
  margin-bottom: 24px;
}


@media screen and (min-width: 1024px) {
  main ul {
    columns: 2;
    column-gap: 2rem;
    display: block;
  }
  .tpc__checklist span {
    height: 19px;
  }
  .tpc__checklist > label {
    display: inline-block;
    padding-right: 1rem;
    margin-bottom: 12px;
  }
  .card.card--flipped {
    -webkit-transform: rotateX(0);
    transform: rotateX(0);
  }
  .card .front,
  .card .back {
    -webkit-backface-visibility: visible;
    backface-visibility: visible;
    transition: all 0.3s ease;
  }
  .card .front {
    background: transparent;
    box-shadow: none;
    text-align: left;
    padding-left: 1.5rem;
    z-index: 3;
    pointer-events: none;
  }
  .card .back {
    -webkit-transform: rotatex(0);
    transform: rotatex(0);
    justify-content: flex-end;
    padding-right: 1.5rem;
  }
  .progress,
  .tipsSwitch,
  .clearProgression {
    transition: all 0.3s ease;
    flex: 0 4rem;
    margin-left: 1.5rem;
  }
}
@media screen and (min-width: 1440px) {
  main ul {
    columns: 3;
  }
}
@media screen and (min-width: 1660px) {
  main ul {
    columns: 4;
  }
}

</style>
