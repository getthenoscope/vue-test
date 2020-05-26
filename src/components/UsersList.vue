<template>
    <div>
        <ul>
        <UsersItem 
        v-for="user of searchUser"
          v-bind:user="user"
          v-bind:searchUser="searchUser"
          :key="user.id"
          v-on:take-user="takeUser"
        />
        </ul>
    </div>
</template>

<script>
import UsersItem from '@/components/UsersItem'
export default {
  props: ['users', 'searchUser'],
  
  methods: {
    takeUser (id) {
      this.$emit('take-user', id)
    }
  },
  components: {
    UsersItem
  },
};
</script>

<style scoped>
ul {
  list-style: none;
  margin: 0;
  padding: 0;
  display: grid;
  grid-template-rows: 1fr 1fr 1fr;
  grid-template-columns: repeat(auto-fill, minmax(100px, 1fr));;
}
</style>