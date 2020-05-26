<template>
        <li>
            <img :src=addUser.avatar_url  alt="" srcset="">
            <span>{{addUser.login}}</span>
            <span class="take" 
                v-on:click="$emit('remove-user', addUser)" 
            >Убрать</span>
        </li>
</template>

<script>
export default {
    props: {
        addUser: {
            type: Object,
            required: true
        }
    }
}
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
img {
    width: 50px;
    border-radius: 50%;
}
li {
    display: grid;
    grid-template-columns: 1fr;  
    grid-template-rows: 1fr 1fr 1fr;
    justify-items: center;
    align-items: center;
}
span {
    color: #fff;
}
.take {
    padding: 5px 10px;
    align-self: start;
    border-radius: 50px;
    background: #9932CC;
    transition: all .3s ease-in-out;
    cursor: pointer;
    border: 1px solid transparent;
}
.take:hover {
    background: transparent;
    border-color: #9932CC;
}  

</style>