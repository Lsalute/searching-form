<template>
    <ul class="tabs">
        <li v-for="tab in data" v-bind:class="{active: tab === selectedTab}" v-on:click="clickTab(tab)">
            {{tab}}
        </li>
    </ul>
</template>

<script>
export default {
    props : ['data', 'selectedTab'],
    methods : {
        clickTab(tabName) {
            this.$emit('@change', tabName);
        },

    }
}
</script>

<style>

</style>

