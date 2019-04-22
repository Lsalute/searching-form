<template>
    <div v-if="data.length">
        <ul class="list">
            <li v-for="item in data">
                <img v-bind:src="item.image">{{item.name}}
            </li>
        </ul>
    </div>
    <div v-else>
        '{{value}}'가 없습니다.
    </div>
</template>

<script>
export default {
    props:['data', 'value']
}
</script>

<style>

</style>
