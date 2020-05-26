<template>
    <li>
        <img @click="info(user.login)" :src=user.avatar_url  alt="" srcset="">
        <span>{{user.login}}</span>
        <span class="take" 
            v-on:click="$emit('take-user', user)"    
        >Выбрать</span>
        <detailInfo
            v-bind:detal="detal"
            v-bind:show="show"
        />
    </li>
</template>

<script>
import detailInfo from '@/components/detailInfo'
export default {
    data () {
        return {
            detal: [],
            show: false
        }
    },
    props: {
        user: {
            type: Object,
            required: true
        }
    },
    methods: {
        info(u) {
        fetch('https://api.github.com/users/' + u)
            .then(response => response.json())
            .then(json => {
              this.detal = json
            })
        this.show = !this.show
        }
    },
    components: {
        detailInfo
    }   
}

</script>

<style  scoped>
    img {
        width: 50px;
        border-radius: 50%;
        cursor: pointer;
    }
    li {
        display: grid;
        grid-template-columns: 1fr;  
        grid-template-rows: 1fr 1fr 1fr;
        justify-items: center;
        align-items: center;
        position: relative;
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