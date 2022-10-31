<template>
    <div id="main-banner">
        <h1>Make Your Burger.</h1>
    </div>
</template>

<script>
export default {
    name: 'Banner',
}
</script>

<style scoped>
    #main-banner {
        background-image: url("/Users/giovanni/Documents/dev/vue/curso-hora-de-codar/3_make_your_burger/public/img/burger.jpg");
        background-position: 0 - 250px;
        background-size: cover;
        width: 100%;
        height: 500px;
        display: flex;
        justify-content: flex-start;
        align-items: center;
    }
    #main-banner h1 {
        margin-left: 3rem;
        color: #FCBA03;
    }
</style>