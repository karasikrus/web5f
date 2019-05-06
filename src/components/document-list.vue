<template>
    <div class="document-list">
        <div v-if="isLoading">loading</div>
        <div v-if="isErrored">error</div>
        <ul>
            <li v-for="document in documents" v-bind:key="document._id" v-bind:class="{ selected: document._id === selectedId}" @click="clicked(document)">
                {{document.title}}
            </li>
        </ul>
    </div>
</template>

<script>

    import axios from "axios";

    export default {
        name: "document-list",
        props: {
            selectedId: String
        },
        data() {
            return {
                isLoading: true,
                isErrored: false,
                documents: [],
            }
        },
        methods: {
            clicked: function (doc) {
                this.$emit('selected-changed', doc._id)
            },
            refreshList() {
                this.isLoading = true;
                this.isErrored = false;
                this.documents = [];
                axios.get('https://web-5.herokuapp.com/documents')
                    .then(response => this.documents = response.data)
                    .catch(() => this.isErrored = true)
                    .finally(() => this.isLoading = false)
            }
        },
        created() {
            this.refreshList();
        }
    }
</script>

<style scoped>
    .selected{
        color: black;
        background-color: brown;
    }

</style>
