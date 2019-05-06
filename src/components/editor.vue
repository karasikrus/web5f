<template>
    <div id="editor" v-if="selectedId">
        <div class="content">
            <label>
                <textarea :value="input" @input="update" :disabled="isDisabled" rows="15" cols="50"></textarea>
             </label>
            <div v-html="compiledMarkdown"></div>
        </div>
        <div class="buttons">
            <button @click="save">Сохранить</button>
            <button @click="deleteNote">Удалить</button>
        </div>
    </div>



</template>

<script>

    import _ from 'lodash';
    import marked from 'marked';
    import axios from "axios";
    import qs from 'qs'

    export default {
        name: "editor",
        props: {
            selectedId: String
        },
        data() {
            return {
                input: "",
                isDisabled: true,
                documents: []
            };
        },
        computed: {
            compiledMarkdown: function () {
                return marked(this.input, {sanitize: true})
            }
        },
        methods: {
            update: _.debounce(function (e) {
                this.input = e.target.value
            }, 300),
            save() {
                const requestBody = qs.stringify({
                    text: this.input
                });
                const config = {
                    headers: {
                        'Content-Type': 'application/x-www-form-urlencoded'
                    }
                };
                axios.put('https://web-5.herokuapp.com/documents/' + this.selectedId, requestBody, config)
                    .then(() => {
                        this.$emit('refresh-list');
                    })
            },
            deleteNote() {
                axios.delete('https://web-5.herokuapp.com/documents/' + this.selectedId)
                    .then(() => {
                        this.$emit('refresh-list');
                        this.$emit('selected-changed', "");
                    })
            }
        },
        watch: {
            selectedId: {
                immediate: true,
                handler(newVal) {
                    this.isDisabled = true;
                    this.input = '';
                    if (newVal) {
                        this.input = '*Loading...*';
                        axios.get('https://web-5.herokuapp.com/documents/' + newVal)
                            .then(response => {
                                if (response.data.text)
                                    this.input = response.data.text;
                                else
                                    this.input = "";
                            })
                            .catch(() => this.input = "# Error")
                            .finally(() => this.isDisabled = false)
                    }
                }
            }
        }
    }
</script>

<style scoped>

</style>
