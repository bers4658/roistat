<template>
  <div id="app">
    <div>
      <UsersTable 
       @showModal='showFormModal'
       :treeUsers = "treeUsers"
      ></UsersTable>
      <Profile
        v-if="isShowModal"
        @closeModal='closeFormModal'
        @saveUser="saveFormUser($event)"
        :bosses="shortDataUsers"
      ></Profile>  
    </div>     
  </div>
</template>

<script>
import UsersTable from './components/UsersTable.vue'
import Profile from './components/Profile.vue'

export default {
  name: 'App',
  components: {
    UsersTable,
    Profile
  },

  created() {
    if (localStorage.getItem('treeUsers')) {
        this.treeUsers = JSON.parse(localStorage.getItem('treeUsers'));
        this.shortDataUsers = JSON.parse(localStorage.getItem('shortDataUsers'));
    }
  },

  data() {
    return{
      isShowModal:false,
      treeUsers: [],
      shortDataUsers: []
    }
  },

  methods: {
    closeFormModal() {
      this.isShowModal = false
    },

    showFormModal() {
      this.isShowModal = true
    },

    saveFormUser(saveData) {
      let newUser = saveData.user
      if(!saveData.boss){ 
        this.treeUsers.push(newUser)
      }else{
        let shortDataBoss = saveData.boss
        let boss = this.findFullUser(this.treeUsers, shortDataBoss.id)
        if (boss == null) {
          console.error("Начальник не найден в дереве пользователей")
          return
        }
        boss.workers.push(newUser)
      }
      let shortDataUser = {
        id: newUser.id,
        name: newUser.name
      }
      this.shortDataUsers.push(shortDataUser);
      this.saveUserLocalStorage()
    },

    findFullUser(users, id) {
      for (let i = 0; i < users.length; i++) {
        if (users[i].id === id) {
          return users[i]
        }
        let workers = users[i].workers
        if (workers.length > 0) {
          let findUser = this.findFullUser(workers, id)
          if (findUser != null) {
            return findUser
          }
        }
      }
      return null
    },

    saveUserLocalStorage(){
      const parsedTreeUsers = JSON.stringify(this.treeUsers);
      localStorage.setItem('treeUsers', parsedTreeUsers);
      const parsedShortDataUsers = JSON.stringify(this.shortDataUsers);
      localStorage.setItem('shortDataUsers', parsedShortDataUsers);
    },
  },
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center; 
}
</style>