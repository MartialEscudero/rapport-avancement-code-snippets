<template>
  <div>
    <!-- Choix mois & année -->
    <div class="my-8 float-right">
      <select 
        class="date_select mr-8 font-bold outline-none bg-gray-900 py-1.5 pr-8 pl-4 cursor-pointer" 
        v-model="monthSelect"
        @change="hiddenOptions(), calendarSetup([yearSelect, monthSelect])"
      >
        <option
          class="font-bold bg-gray-700" 
          v-for="month in monthsList" :key="month.item" 
          :value="month.value"
        >{{ month.name }}</option>
      </select>
      <select 
        class="date_select font-bold outline-none bg-gray-900 py-1.5 pr-8 pl-4 cursor-pointer" 
        v-model="yearSelect" 
        @change="hiddenOptions(), calendarSetup([yearSelect, monthSelect])"
      >
        <option
          class="font-bold bg-gray-700" 
          v-for="year in yearsList" :key="year.item" 
          :value="year"
        >{{ year }}</option>
      </select>
    </div>

    <!-- Calendrier -->
    <div>
      <table class="w-full table-fixed">
        <thead>
          <tr>
            <th 
              class="text-sm py-1.5 text-center bg-gray-800 border-l border-r border-gray-900" 
              v-for="day in daysList" :key="day.item"
            >{{ day.dateNumber }}</th>
          </tr>
        </thead>
        <tbody>
          <tr>
            <td v-for="day in daysList" :key="day.item">
              <div v-if="!day.weekEnd">
                <div class="mt-4">
                  <div
                    class="m-auto w-4 h-4 rounded-full bg-gray-900 border border-white cursor-pointer" 
                    :style="setColorCode(day.project.am)" 
                    @click="hiddenOptions(), displayOption(day.date + '-am')"
                  ></div>
                  <div 
                    class="option hidden fixed mt-3 bg-gray-700 border border-white" 
                    :data-id="day.date + '-am'"
                  >
                    <ul>
                      <li
                        class="flex items-center cursor-pointer py-2 pr-16 pl-4 hover:bg-gray-800" 
                        v-for="project in day.projectsListUser" :key="project.item" 
                        @click="hiddenOptions(), setProject(['am', project.code, day])"
                      >
                        <span
                          class="block w-4 h-4 mr-3 -mt-0.5 rounded-full bg-gray-900 border border-white"
                          :style="setColorCode(project.code)" 
                        ></span>{{ project.code }} - {{ project.name }}
                      </li>
                      <li 
                        class="flex items-center cursor-pointer py-2 pr-16 pl-4 hover:bg-gray-800" 
                        @click="hiddenOptions(), setProject(['am', null, day])"
                      >
                        <span class="block w-4 h-4 mr-3 -mt-0.5 rounded-full bg-gray-900 border border-white"></span>Aucun
                      </li>
                    </ul>
                  </div>
                </div>
                <div class="mt-4">
                  <div 
                    class="m-auto w-4 h-4 rounded-full bg-gray-900 border border-white cursor-pointer" 
                    :style="setColorCode(day.project.pm)"
                    @click="hiddenOptions(), displayOption(day.date + '-pm')"
                  ></div>
                  <div 
                    class="option hidden fixed mt-3 bg-gray-700 border border-white" 
                    :data-id="day.date + '-pm'"
                  >
                    <ul>
                      <li
                        class="flex items-center cursor-pointer py-2 pr-16 pl-4 hover:bg-gray-800" 
                        v-for="project in day.projectsListUser" :key="project.item" 
                        @click="hiddenOptions(), setProject(['pm', project.code, day])"
                      >
                        <span 
                          class="block w-4 h-4 mr-3 -mt-0.5 rounded-full bg-gray-900 border border-white"
                          :style="setColorCode(project.code)"
                        ></span>{{ project.code }} - {{ project.name }}
                      </li>
                      <li
                        class="flex items-center cursor-pointer py-2 pr-16 pl-4 hover:bg-gray-800" 
                        @click="hiddenOptions(), setProject(['pm', null, day])"
                      >
                        <span class="block w-4 h-4 mr-3 -mt-0.5 rounded-full bg-gray-900 border border-white"></span>Aucun
                      </li>
                    </ul>
                  </div>
                </div>
              </div>
              <div v-else>
                <div class="block m-auto mt-4 w-4 h-4 rounded-full bg-gray-700 cursor-not-allowed"></div>
                <div class="block m-auto mt-4 w-4 h-4 rounded-full bg-gray-700 cursor-not-allowed"></div>
              </div>
            </td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Calendrier',
  props: {
    projectsListUser : Array,
    daysListUser: Array,
  },
  data: () => ({
    am: null,
    pm: null,
    yearSelect: null,
    monthSelect: null,
    listExtract: [],
    daysList: [],
    yearsList: [],
    monthsList: [
      {name: 'Janvier', value: 1},
      {name: 'Février', value: 2},
      {name: 'Mars', value: 3},
      {name: 'Avril', value: 4},
      {name: 'Mai', value: 5},
      {name: 'Juin', value: 6},
      {name: 'Juillet', value: 7},
      {name: 'Aout', value: 8},
      {name: 'Septembre', value: 9},
      {name: 'Octobre', value: 10},
      {name: 'Novembre', value: 11},
      {name: 'Décembre', value: 12},
    ],
    projectsList : [
      {code: "SOIX09", color_code:"#84CC16", name: "Lippi"},
      {code: "SOSU09", color_code:"#EAB308;", name: "BatiRIM"},
      {code: "SOCA03", color_code:"#EF4444;", name: "Canal+"},
    ],
  }),
  methods: {
    toTimestamp(item) {
      return new Date(item[0], item[1] - 1, item[2]).getTime() / 1000
    },

    toDate(item) {
      return (new Date(item * 1000))
    },

    indexCloserTimestamp(item) {
      return this.daysListUser.indexOf(this.daysListUser.find(element => element.date >= item))
    },

    indexListExtract(item) {
      return this.listExtract.map(function(e) { return e.date }).indexOf(item)
    },

    indexDaysListUser(item) {
      return this.daysListUser.map(function(e) { return e.date }).indexOf(item)
    },

    dateNumberTest(item) {
      if (item < 10) { return "0" + item } else { return item }
    },

    weekEndTest(item) {
      var day = new Date(this.toDate(item)).getDay()
      if (day === 6 || day === 0) { return true } else { return false }
    },

    sortDaysListUser() {
      this.daysListUser.sort(function (a, b) { return a.date - b.date })
    },

    setColorCode(item) {
      if (!!item) {
        var index = this.projectsList.map(function(e) { return e.code }).indexOf(item)
        var color =  this.projectsList[index].color_code
        return 'background-color:' + color + ';border:none;'
      }
    },

    hiddenOptions() {
      var list = document.querySelectorAll(".option")
      for (var i = 0; i < list.length; ++i) {
        list[i].classList.add('hidden');
      }
    },

    currentDate() {
      // récupère la date actuelle au chargement de la page
      var date = new Date()
      this.yearSelect = date.getFullYear()
      this.monthSelect = date.getMonth() + 1

      // liste les années - actuelle + 2 précédentes
      for (let i = 0; i < 3; i++) {
        this.yearsList.push(this.yearSelect - i)
      }
    },

    calendarSetup(item) {
      // donne le nombre de jours dans le mois sélectionné
      var monthStart = new Date(item[0], item[1] - 1, 1)
      var monthEnd = new Date(item[0], item[1], 1)
      var nbDay = Math.round((monthEnd - monthStart) / (1000 * 60 * 60 * 24))

      // isoler les timestamp du mois sélectionné
      this.sortDaysListUser()
      var calendarStart = this.toTimestamp([item[0], item[1], 1])
      var calendarEnd = this.toTimestamp([item[0], item[1] + 1, 1])
      if (this.indexCloserTimestamp(calendarEnd) < 0) {
        this.listExtract = this.daysListUser.slice(this.indexCloserTimestamp(calendarStart))
      } else {
        this.listExtract = this.daysListUser.slice(this.indexCloserTimestamp(calendarStart), this.indexCloserTimestamp(calendarEnd))
      }

      // liste les jours du mois sélectionné, définit si c'est un jour du w-e ou non
      this.daysList= []
      for (let i = 1; i <= nbDay; i++) {
        var timestamp = this.toTimestamp([this.yearSelect,this.monthSelect, i])
        if (this.indexListExtract(timestamp) != -1) {
          var index = this.indexListExtract(timestamp)
          this.daysList.push({
            date: timestamp,
            project: this.listExtract[index].project,
            projectsListUser: this.projectsListUser,
            dateNumber: this.dateNumberTest(i), 
            weekEnd: this.weekEndTest(timestamp),
          })
        } else {
          this.daysList.push({
            date: timestamp,
            project: { am: null, pm: null },
            projectsListUser: this.projectsListUser,
            dateNumber: this.dateNumberTest(i), 
            weekEnd: this.weekEndTest(timestamp),
          })
        }
      }
    },

    // masque le dropdown
    displayOption(item) {
      document.querySelector("[data-id="+"'"+item+"'"+"]").classList.toggle("hidden")
    },

    // ajoute le projet sélectionné
    setProject(item) {
      var index = this.indexDaysListUser(item[2].date)
      if (index >= 0) {
        this.daysListUser[index].project[item[0]] = item[1]
      } else {
        this.daysListUser.push({
          date: item[2].date, 
          project: { [item[0]]: item[1] }
        })
      }
      this.calendarSetup([this.yearSelect,this.monthSelect])
    },
  },
  mounted() {
    this.currentDate()
    this.calendarSetup([this.yearSelect,this.monthSelect])
  },
}
</script>

<style scoped>
.date_select {
  border: 1px solid white;
  -moz-appearance:none;
  -webkit-appearance:none;
  appearance:none;
  background: url("data:image/svg+xml,<svg fill='white' height='24' viewBox='5 0 24 24' width='24' xmlns='http://www.w3.org/2000/svg'><path d='M7 10l5 5 5-5z'/><path d='M0 0h24v24H0z' fill='none'/></svg>") no-repeat right;
}
</style>