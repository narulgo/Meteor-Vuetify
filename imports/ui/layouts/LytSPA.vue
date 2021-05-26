<template>
    <v-container fluid>
        <header-view></header-view>
        <v-content id="main_section">
            <router-view name="sectionView"></router-view>
        </v-content>
        <footer-view></footer-view>
    </v-container>
</template>

<script>
    import HeaderView from "./shared/HeaderView";
    import FooterView from "./shared/FooterView";

    export default {
        name: "LytSPA",
        components: {
            HeaderView,
            FooterView
        }
    }
</script>

<style scoped>
    #main_section {
        margin-bottom: 80px;
        position: relative;
    }
</style>