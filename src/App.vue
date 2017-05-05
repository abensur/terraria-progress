<template>
  <div class="tpc">
    <h1>Terraria Progression</h1>
    <div class="tpc__info" v-bind:class="{ 'tpc__info--active': progressionPercentage !== 0 }">
      <el-progress type="circle"
        :width="75"
        :percentage="progressionPercentage"
        :status="progressionPercentage === 100 ? 'success' : ''"></el-progress>
      <el-button @click="clearProgression" type="danger"><b> {{ progressionTotal  }} / {{ progressionData.length }} </b><i class="el-icon-circle-cross"></i></el-button>
    </div>

    <div class="tpc__checklist">
        <el-checkbox
          v-for="(item, index) in progressionData"
          :key="index"
          @change="onToggle(item.key)"
          v-model="progression[item.key]">
          <span><b>{{ item.label }}</b><small> - {{ item.desc }}</small></span>
        </el-checkbox>
    </div>
  </div>
</template>
<script>
import PROGRESSION from '@/progression.json'

export default {
  name: 'app',
  data () {
    const progression = PROGRESSION.reduce((acc, cur) => {
      acc[cur.key] = false
      return acc
    }, {})

    return {
      progression,
      progressionTotal: 0,
      progressionData: PROGRESSION
    }
  },
  computed: {
    'progressionPercentage' () {
      return Math.round((this.progressionTotal / this.progressionData.length) * 100)
    }
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
    }
  }
}
</script>
<style>
* { box-sizing: border-box; }
html, body {
  height: 100%;
  margin: 0;
  padding: 0;
}
.tpc {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  height: 100%;
}
.tpc > h1 {
  text-align: center;
  font-size: 1.5rem;
  height: 100px;
  margin: 0 0 1.5rem;
  line-height: 100px;
}
.tpc__info {
  position: fixed;
  transform: translate(0, -100%);
  transition: transform 0.3s ease, width 0.3s ease;
  left: 0;
  right: 0;
  top: 0;
  background: #fff;
  z-index: 2;
  height: 100px;
  display: flex;
  flex-flow: row wrap;
  align-items: center;
  justify-content: space-around;
  box-shadow: 0 1px 3px rgba(0,0,0,0.12), 0 1px 2px rgba(0,0,0,0.24);
}
.tpc__info--active {
  transform: translate(0, 0);
}
.tpc__info > div,
.tpc__info > button {
  flex: 0 auto;
}
.tpc__checklist {
  width: 100%;
  display: flex;
  flex-flow: row wrap;
  padding: 0 1.5rem 1.5rem;
}
.tpc__checklist span {
  transition: opacity 0.6s ease;
}
.is-checked+span {
  font-style: italic;
  color: rgb(19, 206, 102);
  text-decoration: line-through;
  opacity: 0.5;
}
.is-checked+span > span { color: #2c3e50; }
.tpc__checklist > label {
  flex: 0 100%;
  margin-left: 0 !important;
  white-space: normal;
  margin-bottom: 24px;
}
@media screen and (min-width: 1024px) {
  html {
    font-size: 24px;
  }
  .tpc__info {
    left: auto;
    width: 320px;
    transform: translate(0, 0);
  }
  .tpc__checklist {
    width: 1024px;
    height: auto;
    margin: 0 auto;
    display: block;
    columns: 2;
    column-gap: 2rem;
  }
  .tpc__checklist span {
    height: 19px;
  }
  .tpc__checklist > label {
    display: inline-block;
    padding-right: 1rem;
    margin-bottom: 12px;
  }
}
</style>
