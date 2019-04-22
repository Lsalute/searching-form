<template>
    <div v-if="data.length">
        <ul class="list">
            <li v-for="(item,index) in data" v-on:click="onClickList(item.keyword)">
                <span class="number" v-if="keywordType">{{index + 1}}</span>
                <span>{{item.keyword}}</span>
                <span v-if="historyType" class="date">{{item.date}}</span>
                <button v-if="historyType" class="btn-remove" v-on:click.stop="onRemoveList(item.keyword)"></button>
            </li>
        </ul>
    </div>
    <div v-else>
        <span v-if="keywordType">추천 검색어가 없습니다.</span>
        <span v-if="historyType">최근 검색어가 없습니다.</span>
    </div>
</template>

<script>
export default {
    props:['data','type'],
    computed : {
        keywordType() {
            return this.type === "keyword"
        },
        historyType() {
            return this.type === "history"
        }
    },
    methods: {
        onClickList(value) {
            this.$emit('@click', value)
        },
        onRemoveList(value) {
            this.$emit('@remove', value)
        }
    }
}
</script>

<style>

</style>
