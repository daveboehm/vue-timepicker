<template>
    <div id="app">
        <svg aria-hidden="true" style="position: absolute; width: 0; height: 0; overflow: hidden;" version="1.1" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink">
            <defs>
                <symbol id="icon-time" viewBox="0 0 20 20">
                    <title>time</title>
                    <path d="M10 20c-5.523 0-10-4.477-10-10s4.477-10 10-10v0c5.523 0 10 4.477 10 10s-4.477 10-10 10v0zM10 18c4.418 0 8-3.582 8-8s-3.582-8-8-8v0c-4.418 0-8 3.582-8 8s3.582 8 8 8v0zM9 10.41v-6.41h2v5.59l3.95 3.95-1.41 1.41-4.54-4.54z"></path>
                </symbol>
            </defs>
        </svg>
        <section class="section">
            <h2>Timepicker</h2>
            <timepicker :label="'Pick a time!'" />
        </section>
    </div>
</template>

<script>
    import Timepicker from './components/Timepicker.vue';
    
    export default {
        name: 'app',
        components: {
            'timepicker': Timepicker
        }
    }
</script>

<style lang="scss">
    #app {
        font-family: 'Avenir', Helvetica, Arial, sans-serif;
    }
    
    .section {
        width: 100%;
        max-width: 600px;
        margin: 40px auto;
    }
</style>
