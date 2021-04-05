<template>
<div class="add">
    <h3>Add a new announcement/event</h3>
    <form id="newEventForm">
        <div class="row">
            <label for="title">Title: <input v-model="title" type="text" name="title" class="pure-input-rounded"></label>
        </div>
        <div class="row">
            <label for="description">Announcement/Event Description: <input v-model="description" type="text" name="description"></label>
        </div>
        <div class="row">
            <label for="location">Where will this take place? <input v-model="location" type="text" name="location"></label>
        </div>
        <div class="row">
            <label for="date">When will this take place? <input v-model="date" type="date" name="date"></label>
        </div>
        <div class="row">
            <button class="button-primary" v-on:click="add()" type="button">Add Annoucement/Event</button>
        </div>
    </form>
</div>
</template>

<script>
import axios from 'axios';
    
export default {
    name: "Add",
    data() {
        return {
            title: "",
            description: "",
            location: "",
            date: "",
        }
    },
    methods: {
        async add() {
            try {
                await axios.post("/api/events", {
                    title: this.title,
                    description: this.description,
                    location: this.location,
                    date: this.date,
                });
                
                this.$router.push('Board');
            } catch (error) {
                console.log(error);
            }
        }
    }
}
</script>