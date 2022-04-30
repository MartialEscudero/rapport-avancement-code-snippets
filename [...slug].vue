<template>
  <div>
    <div>
      <!-- Liste des projets -->
      <div class="flex items-center justify-between w-full bg-cyan-400 py-3 px-5 mt-8 font-bold">
        <h1 class="text-base">Liste des projets</h1>
        <img 
          class="cursor-pointer transform" 
          src="~assets/img/arrow.png" 
          @click="e => e.target.classList.toggle('rotate-180') & ((!showProjets) ? showProjets = true : showProjets = false)"
        />
      </div>
      <div class="w-full bg-gray-700 py-4" v-if="!!showProjets">
        <div class="container grid grid-cols-6 gap-4">
          <div class="flex items-center m-auto" v-for="project in projectsListUser" :key="project.item">
            <span
              class="block w-4 h-4 mr-3 -mt-0.5 rounded-full bg-gray-900 border boder-white" 
              :style="setColorCode(project.code)"
            ></span>
            <p class="text-sm">
              {{ project.code }} - {{ project.name }}
            </p>
          </div>
        </div>
      </div>

      <!-- Calendrier -->
      <CalendarEditor :projectsListUser="projectsListUser" :daysListUser="daysListUser" />

      <!-- Enregistrer -->
      <div class="mt-44 text-center">
        <a class="text-sm px-24 py-3 bg-lime-500 transition ease-in-out cursor-pointer hover:bg-lime-600" @click="saveCalendar()">Enregistrer</a>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  head: () => ({
    title: 'John Doe',
  }),
  data: () => ({
    showProjets: false,
    // données en durs en attendant la bdd
    projectsList: [
      {code: "SOIX09", color_code:"#84CC16", name: "Lippi"},
      {code: "SOSU09", color_code:"#EAB308;", name: "BatiRIM"},
      {code: "SOCA03", color_code:"#EF4444;", name: "Canal+"},
    ],
    projectsListUser: [
      {code: "SOIX09", color_code:"#84CC16", name: "Lippi"},
      {code: "SOSU09", color_code:"#EAB308;", name: "BatiRIM"},
    ],
    daysListUser: [
      {date: 1641164400, project: {am : "SOIX09", pm: "SOSU09"}}, // 3 janvier
      {date: 1648764000, project: {am : "SOIX09", pm: "SOSU09"}}, // 1 avril
      {date: 1649023200, project: {am : "SOIX09", pm: "SOSU09"}}, // 4 avril
      {date: 1649109600, project: {am : "SOIX09", pm: "SOSU09"}}, // 5 avril
      {date: 1649196000, project: {am : "SOIX09", pm: "SOSU09"}}, // 6 avril
      {date: 1649282400, project: {am : "SOIX09", pm: "SOSU09"}}, // 7 avril
      {date: 1649368800, project: {am : "SOIX09", pm: "SOSU09"}}, // 8 avril
      {date: 1649628000, project: {am : "SOSU09", pm: "SOIX09"}}, // 11 avril
      {date: 1651442400, project: {am : "SOIX09", pm: "SOSU09"}}, // 2 mai
      {date: 1655848800, project: {am : "SOIX09", pm: "SOSU09"}}, // 22 juin
    ]
  }),
  methods: {
    setColorCode(item) {
      if (!!item) {
        var index = this.projectsList.map(function(e) { return e.code }).indexOf(item)
        var color =  this.projectsList[index].color_code
        return 'background-color:' + color + ';border:none;'
      }
    },

    // enregistrement du calendrier
    saveCalendar() {},
  },
}
</script>