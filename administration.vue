<template>
  <div>
    <ModalEditUsers />
    <ModalEditProjects />
    <div class="grid grid-cols-4 gap-10 mt-16">
      <div class="text-center bg-gray-800 border-2 border-gray-700 py-4">
        <span class="text-4xl font-bold">10</span>
        <p class="text-lg mt-3.5">Projets</p>
      </div>
      <div class="text-center bg-gray-800 border-2 border-gray-700 py-4">
        <span class="text-4xl font-bold">10</span>
        <p class="text-lg mt-3.5">Projets en cours</p>
      </div>
      <div class="text-center bg-gray-800 border-2 border-gray-700 py-4">
        <span class="text-4xl font-bold">10</span>
        <p class="text-lg mt-3.5">Projets terminés</p>
      </div>
      <div class="text-center bg-gray-800 border-2 border-gray-700 py-4">
        <span class="text-4xl font-bold">10</span>
        <p class="text-lg mt-3.5">Collaborateurs</p>
      </div>
    </div>
    <div class="flex gap-32">
      <div class="grow-2">
        <div class="flex justify-between items-center mt-20 mb-5">
          <h1 class="text-lg font-bold">Liste des collaborateurs</h1>
          <a 
            class="rounded-full px-2.5 cursor-pointer bg-lime-500 hover:bg-lime-600 text-4xl transition ease-in-out" 
            data-modal-toggle="modal-users"
          >+</a>
        </div>
        <table class="border-separate w-full">
          <thead>
            <tr class="table-row text-sm text-left">
              <th class="border-b border-gray-500 table-cell py-1.5">Nom</th>
              <th class="border-b border-gray-500 table-cell py-1.5">Prénom</th>
              <th class="border-b border-gray-500 table-cell py-1.5">Adresse Mail</th>
              <th class="border-b border-gray-500 table-cell py-1.5">Date d’entrée</th>
              <th class="border-b border-gray-500 table-cell py-1.5">Date de sortie</th>
              <th class="border-b border-gray-500 table-cell py-1.5">Actions</th>
            </tr>
          </thead>
          <tbody 
            class="table-row-group" 
            v-for="user in users" :key="user.item"
          >
            <tr class="table-row text-sm text-left ">
              <td class="table-cell py-1.5">{{ user.name }}</td>
              <td class="table-cell py-1.5">{{ user.firstname }}</td>
              <td class="table-cell py-1.5">{{ user.email }}</td>
              <td class="table-cell py-1.5">{{ user.startAt }}</td>
              <td class="table-cell py-1.5">{{ user.endAt }}</td>
              <td class="table-cell py-1.5">
                <svg xmlns="http://www.w3.org/2000/svg" data-modal-toggle="modal-users" width="18" height="18" fill="currentColor" class="cursor-pointer" viewBox="0 0 16 16">
                  <path d="M12.854.146a.5.5 0 0 0-.707  0L10.5 1.793 14.207 5.5l1.647-1.646a.5.5 0 0 0 0-.708l-3-3zm.646 6.061L9.793 2.5 3.293 9H3.5a.5.5 0 0 1 .5.5v.5h.5a.5.5 0 0 1 .5.5v.5h.5a.5.5 0 0 1 .5.5v.5h.5a.5.5 0 0 1 .5.5v.207l6.5-6.5zm-7.468 7.468A.5.5 0 0 1 6 13.5V13h-.5a.5.5 0 0 1-.5-.5V12h-.5a.5.5 0 0 1-.5-.5V11h-.5a.5.5 0 0 1-.5-.5V10h-.5a.499.499 0 0 1-.175-.032l-.179.178a.5.5 0 0 0-.11.168l-2 5a.5.5 0 0 0 .65.65l5-2a.5.5 0 0 0 .168-.11l.178-.178z"/>
                </svg>   
              </td>
            </tr>
          </tbody>
        </table>
      </div>
      <div class="grow">
        <div class="flex justify-between items-center mt-20 mb-5">
          <h1 class="text-lg font-bold">Liste des projets</h1>
          <a 
            class="rounded-full px-2.5 cursor-pointer bg-lime-500 hover:bg-lime-600 text-4xl transition ease-in-out"
            data-modal-toggle="modal-projects"
          >+</a>
        </div>
        <table class="border-separate w-full">
          <thead>
            <tr class="table-row text-sm text-left">
              <th class="border-b border-gray-500 table-cell py-1.5">Code</th>
              <th class="border-b border-gray-500 table-cell py-1.5">Nom</th>
              <th class="border-b border-gray-500 table-cell py-1.5">NHP</th>
              <th class="border-b border-gray-500 table-cell py-1.5">NHR</th>
              <th class="border-b border-gray-500 table-cell py-1.5">Actions</th>
            </tr>
          </thead>
          <tbody 
            class="table-row-group" 
            v-for="project in projects" :key="project.item"
          >
            <tr class="table-row text-sm text-left ">
              <td class="table-cell py-1.5">
                <span 
                  class="block w-4 h-4 mr-2 rounded-full float-left" 
                  :style="'background-color:' + project.color_code"
                ></span>{{project.code}}
              </td>
              <td class="table-cell py-1.5">{{ project.name }}</td>
              <td class="table-cell py-1.5">{{ project.nhp }}</td>
              <td class="table-cell py-1.5">{{ project.nhr }}
                <span 
                  class="block w-4 h-4 mr-2 rounded-full float-left bg-red-500" 
                  v-if="project.nhp < project.nhr"
                ></span>
                <span 
                  class="block w-4 h-4 mr-2 rounded-full float-left bg-lime-500" 
                  v-else
                ></span>
              </td>
              <td class="table-cell py-1.5">
                <svg xmlns="http://www.w3.org/2000/svg" data-modal-toggle="modal-projects" width="18" height="18" fill="currentColor" class="cursor-pointer" viewBox="0 0 16 16">
                  <path d="M12.854.146a.5.5 0 0 0-.707 0L10.5 1.793 14.207 5.5l1.647-1.646a.5.5 0 0 0 0-.708l-3-3zm.646 6.061L9.793 2.5 3.293 9H3.5a.5.5 0 0 1 .5.5v.5h.5a.5.5 0 0 1 .5.5v.5h.5a.5.5 0 0 1 .5.5v.5h.5a.5.5 0 0 1 .5.5v.207l6.5-6.5zm-7.468 7.468A.5.5 0 0 1 6 13.5V13h-.5a.5.5 0 0 1-.5-.5V12h-.5a.5.5 0 0 1-.5-.5V11h-.5a.5.5 0 0 1-.5-.5V10h-.5a.499.499 0 0 1-.175-.032l-.179.178a.5.5 0 0 0-.11.168l-2 5a.5.5 0 0 0 .65.65l5-2a.5.5 0 0 0 .168-.11l.178-.178z"/>
                </svg>   
              </td>
            </tr>
          </tbody>
        </table>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Administration',
  head() {
    return {
      title: 'Administration',
    }
  },
  data: () => ({
    editProjectsDialog : false,
    editUsersDialog : false,
    users : [
      {name: "Doe", firstname: "John", email: "jdoe@nexton-group.com", startAt: "01/01/2022", endAt: "01/01/2022"},
      {name: "Doe", firstname: "John", email: "jdoe@nexton-group.com", startAt: "01/01/2022", endAt: "01/01/2022"},
      {name: "Doe", firstname: "John", email: "jdoe@nexton-group.com", startAt: "01/01/2022", endAt: "-"},
      {name: "Doe", firstname: "John", email: "jdoe@nexton-group.com", startAt: "01/01/2022", endAt: "-"},
      {name: "Doe", firstname: "John", email: "jdoe@nexton-group.com", startAt: "01/01/2022", endAt: "01/01/2022"}
    ],
    projects : [
      {code: "SOIX09", color_code:"#84CC16", name: "Lippi", nhp: "50", nhr: "55"},
      {code: "SOSU09", color_code:"#EAB308;", name: "BatiRIM", nhp: "48", nhr: "39"},
    ]
  }),
}
</script>