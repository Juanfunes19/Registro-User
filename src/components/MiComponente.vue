<template lang="">
    <div>
        <h1>{{personajes}}</h1>
        
    </div>
</template>

<script>
// import axios from 'axios'
export default {
    name: "MiComponente",
    data() {
    return {
        personajes: null
    };
    },
    mounted () {
        console.log('Esto anda')
        this.getPersonajes()
    },
};
</script>

<style lang=""></style>
