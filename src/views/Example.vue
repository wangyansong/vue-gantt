<template>
  <div>
    <div id="ganttastic-wrapper">
      <g-gantt-chart
        :chart-start="chartStart"
        :chart-end="chartEnd"
        :grid="grid"
        :hide-timeaxis="hideTimeaxis"
        :push-on-overlap="pushOnOverlap"
        :highlighted-hours="highlightedHours"
        :row-label-width="`${rowLabelWidth}%`"
        :row-height="rowHeight"
        :theme="selectedTheme"
        @contextmenu-bar="onContextmenuBar($event)"
        @dragend-bar="stoppedDraggingBar($event)"
      >
        <g-gantt-row
          v-for="row in rowList"
          :key="row.title"
          :label="row.label"
          :bars="row.barList"
          :highlight-on-hover="highlightOnHover"
          bar-start="myStart"
          bar-end="myEnd"
        >
          <template #bar-label="{bar}">
            <img
              v-if="bar.image"
              :src="require(`@/assets/${bar.image}`)"
              height="20"
              width="20"
              class="mr-1"
            >
            <span>{{bar.label}}</span>
          </template>
        </g-gantt-row>
      </g-gantt-chart>
    </div>

    <v-menu
      v-model="showContextmenu"
      :position-x="contextmenuX"
      :position-y="contextmenuY"
    >
      <v-list>
        <v-list-item>
          It's a context menu!
        </v-list-item>
      </v-list>
    </v-menu>

  </div>
</template>

<script>
import {GGanttChart, GGanttRow} from '../components/gantt'

export default {
  components: {
    GGanttChart,
    GGanttRow
  },

  data(){
    return {
      chartStart: "2020-01-01 00:00",
      chartEnd: "2020-01-01 09:00",
      pushOnOverlap: true,
      grid: true,
      rowHeight: 40,
      rowLabelWidth: 15,
      hideTimeaxis: false,
      highlightOnHover: false,
      hours: [...Array(24).keys()],
      highlightedHours: [],
      showContextmenu: false,
      contextmenuTimeout: null,
      contextmenuX: 0,
      contextmenuY: 0,
      selectedTheme: "default",
      themes: [
        "default",
        "vue",
        "dark",
        "material-blue",
        "creamy",
        "slumber",
        "sky",
        "crimson",
        "grove",
        "fuchsia",
        "flare"
      ],
      rowList: [
        {
          label: "Row #1",
          barList: [
            {
              myStart: "2020-01-01 00:00",
              myEnd: "2020-01-01 00:03",
              label: "xi菜",
              ganttBarConfig: {color:"white", backgroundColor: "#2e74a3"}
            },
            {
              myStart: "2020-01-01 01:15",
              myEnd: "2020-01-01 02:00",
              label: "背菜",
              ganttBarConfig: {color:"white", backgroundColor: "#2e74a3"}
            },
            {
              myStart: "2020-01-01 04:00",
              myEnd: "2020-01-01 06:00",
              label: "烹饪",
              ganttBarConfig: {color:"white", backgroundColor: "#2e74a3", bundle: "blueBundle"}
            }
          ]
        },

        {
          label: "Row #2",
          barList: [
            {
              myStart: "2020-01-01 00:00",
              myEnd: "2020-01-01 01:20",
              label: "800s",
              ganttBarConfig: {color:"white", backgroundColor: "#2e74a3"}
            },
          ]
        },

        {
          label: "Row #3",
          barList: [
          ]
        },

        {
          label: "Row #4",
          barList: [
          ]
        }

      ]
    }
  },


  methods: {

    stoppedDraggingBar(){
      // 30s = 0.5min = 0.5 / 10 * 60 (min)

      /*
      * 00:00:00 =>   2020-01-01 00:00
      * 00:00:30 =>   2020-01-01 00:03
      *
      * */

    },

    // getTime(t) {
    //   let result = "";
    //   return t/10
    // },

    onContextmenuBar(e){
      e.event.preventDefault()
      this.contextmenuY = e.event.clientY
      this.contextmenuX = e.event.clientX
      this.showContextmenu = true
      if(this.contextmenuTimeout){
        clearTimeout(this.contextmenuTimeout)
      }
      this.contextmenuTimeout = setTimeout(() => this.showContextmenu = false, 3000)
    }

  }
}
</script>

<style scoped>
  #ganttastic-wrapper{
    height: 50vh;
    overflow-y: auto;
  }
</style>
