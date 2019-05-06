<template>
        <div class="create">
            <label>
                <input placeholder="Новый документ" id="create-input" required v-model="inputValue">
            </label>
            <button @click="create">Создать</button>
        </div>
</template>

<script>

    import axios from "axios";
    import qs from 'qs';

    export default {
        name: "create-document",
        data() {
            return {
                inputValue: ""
            }
        },
        methods: {
            create() {
                const requestBody = qs.stringify({
                    title: this.inputValue
                });
                const config = {
                    headers: {
                        'Content-Type': 'application/x-www-form-urlencoded'
                    }
                };
                axios.post('https://web-5.herokuapp.com/documents', requestBody, config)
                    .then(response => {
                        this.inputValue = "";
                        this.$emit('refresh-list');
                        this.$emit('selected-changed', response.data)
                    });


            }

        }
    }
</script>

<style scoped>

</style>
