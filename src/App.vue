<template lang="pug">
    div#app
        v-app
            p.text-center.display-1 Hot Dogs
            div.text-right.ma-2
                v-btn(@click="add" color="blue")
                    | New hot dog
            HotDogsList(:hotDogs="hotDogs")
            HotDogForm(:hotDog="hotDog" :formMode="formMode" :formDialog="formDialog" @unshift="unshiftToArray", @edit="editInArr")

</template>

<script>

    import HotDogsList from "./components/HotDogsList";
    import axios from "axios";
    import HotDogForm from "./components/HotDogForm";

    export default {
        name: 'App',
        components: {
            HotDogForm,
            HotDogsList
        },
        provide() {
            return {
                update: () => this.getHotDogs(),
                hotDogAction: (hotDog, formMode, formDialog) => {
                    this.hotDog = {...hotDog};
                    this.formMode = formMode;
                    this.formDialog = formDialog;
                },
                deleteFromArr: (hotDog) => {
                    this.hotDogs = this.hotDogs.filter(item => item._id !== hotDog._id);
                }
            }
        },
        data: () => ({
            hotDogs: [],
            index: 0,
            hotDog: {
                title: ""
            },
            formMode: "",
            formDialog: false,
        }),
        methods: {
            async add() {
                this.formDialog = true;
                this.formMode = 'adding';
            },
            async getHotDogs() {
                let url = 'https://hot-dogs-ao.herokuapp.com/api/getAll';
                try {
                    let response = await axios.get(url);
                    this.hotDogs = response.data.reverse();
                } catch (e) {
                    alert(e.message);
                }
            },
            unshiftToArray(hotDog) {
                this.hotDogs.unshift(hotDog);
            },
            editInArr(hotDog) {
                this.hotDogs.find(item => {
                    if (item._id === hotDog._id)
                        item.title = hotDog.title;
                });
            },
        },
        created() {
            this.getHotDogs();
        },
    };
</script>
<style>
</style>
