<template>
    <form action="" v-on:submit.prevent="onSubmit">
        <input type="text" v-model="inputValue" v-on:keyup="onKeyup" placeholder="검색어를 입력하세요" autofocus>
        <button type="reset" v-show="inputValue.length" v-on:click="onReset" class="btn-reset"></button>
    </form>
</template>


<script>
export default {
    props: ['value'],
    data () {
        return {
            inputValue : this.value
        }
    },
    watch : {
        value(newVal, oldVal){
            this.inputValue = newVal;
        }
    },
    methods: {
        onSubmit(){
            this.$emit('@submit', this.inputValue.trim())
        },
        onKeyup(){
            if(!this.inputValue.length) this.onReset();
        },
        onReset(){
            console.log('[FormComponent] onRest()');
            this.inputValue = '';
            this.$emit('@reset');
        }
    }
}
</script>

<style>

</style>
