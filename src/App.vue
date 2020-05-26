<template>
  <div id="app">
    <div id="header">
       <input type="text" v-model="search" placeholder="Поиск...">
    </div>
    <div id="sidebar">
      <span v-bind:class="{color: currentTab == 'all'}" class="link" @click="currentTab = 'all'">Все пользователи</span>
      <span v-bind:class="{color: currentTab == 'add'}" class="link" @click="currentTab = 'add'">Выбраные пользователи</span>
    </div>
    <UsersList
      id="content"
      v-if="currentTab == 'all'"
      v-bind:users="users"
      v-bind:searchUser="searchUser"
      @take-user="takeUser"
    />
    <div  id="content" v-if="currentTab == 'add'">
    <ul>
      <addUsersList
        v-for="addUser of searchaddUser"
        v-bind:addUser="addUser"
        :key="addUser.id"
        @remove-user="removeUser"
        />
    </ul>
    <span class="clearAll" v-on:click="clearAll">Очистить всё</span>
    </div>
    <div id="footer"></div>
  </div>
</template>

<script>
import UsersList from '@/components/UsersList'
import addUsersList from '@/components/addUsersList';
export default {
  name: 'app',
  data () {
    return {
      users: [],
      addUsers: [],
      currentTab: 'all',
      search: ''
    }
  },
  mounted() {
    fetch('https://api.github.com/users')
    .then(response => response.json())
    .then(json => {
      this.users = json
      
    })
    if (localStorage.getItem('addUsers')) {
      try {
        this.addUsers = JSON.parse(localStorage.getItem('addUsers'));
      } catch(e) {
        localStorage.removeItem('addUsers');
      }
    }    
  },
  computed: {
    searchUser() {
      return this.users.filter(item => item.login.indexOf(this.search) !== -1 || item.id == this.search )
    },
    searchaddUser() {
      return this.addUsers.filter(item => item.login.indexOf(this.search) !== -1 || item.id == this.search)
    }
},
  methods: {
    takeUser(id) {
      if(!this.addUsers.includes(id)) {
        this.addUsers.push(id);
      }
      this.saveUsers()
    },
    removeUser(addUser) {
      this.addUsers = this.addUsers.filter(u => u.id !== addUser.id)
      this.saveUsers()
    },
    saveUsers() {
      const parsed = JSON.stringify(this.addUsers);
      localStorage.setItem('addUsers', parsed);
    },
    clearAll() {
      this.addUsers = [];
      this.saveUsers()
      
    }    
  },
  components: {
    UsersList,addUsersList
  }
};
</script>

<style>
* {
  margin: 0;
  padding: 0;
}
#app {
  display: grid;  
  grid-template-areas: "header header"  
                       "sidebar content"  
                       "footer footer";
  grid-template-columns: 200px 1fr;  
  grid-template-rows: 100px 1fr 100px;
  height: 100vh;
}
#header {
  display: grid;
  background-color: #2b2e40;
  grid-area: header;
}
#sidebar {
  background-color: #242736;
  grid-area: sidebar;
}
#content {

  padding: 3rem  3rem 0 3rem;
  background-color: #1d1e26;
  grid-area: content;
}
#footer {
  background-color: #2b2e40;
  grid-area: footer;
}
ul {
  list-style: none;
    margin: 0;
    padding: 0;
    display: grid;
    grid-template-rows: 1fr 1fr 1fr;
    grid-template-columns: repeat(auto-fill, minmax(100px, 1fr));
}
.clearAll {
  padding: 5px 10px;
  align-self: start;
  border-radius: 50px;
  background: #9932CC;
  transition: all .3s ease-in-out;
  cursor: pointer;
  border: 1px solid transparent;
  color: #fff;
}
.clearAll:hover {
    background: transparent;
    border-color: #9932CC;
}
.link {
  display: grid;
  width: 100%;
  padding: 10px;
  color: #fff;
  cursor: pointer;
}
.color {
  background-color: #9932CC;
}
input {
  width: 200px;
  height: 20px;
  align-self: center;
  justify-self: center;
  border: 0;
  border-radius: 50px;
  padding-left: 20px;
  background-color: #1d1e26;
  color: #fff;
  outline:none;

}
</style>
