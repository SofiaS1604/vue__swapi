<template>
    <div class="page">
        <my-header/>
        <router-view :key="$route.path"/>
    </div>
</template>
<script>
    import MyHeader from "./templates/MyHeader.vue";

    const componentsList = {};
    componentsList[MyHeader.name] = MyHeader;

    export default {
        name: 'App',
        components: componentsList
    };
</script>
