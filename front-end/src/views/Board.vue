<template>
<div class="bulletin_board">
    <a href="/add"><button>Create an announcement/event for the bulletin board</button></a>
    <div v-for="event in events" :key="event.date">
        <div class="announcement" :id="event._id">
            <h3>{{event.title}}</h3>
            <p>What? - {{event.description}}</p>
            <p>When? - {{event.date}}</p>
            <p>Where? - {{event.location}}</p>
            <p>How many people are attending? - {{event.attendee_amount}}</p>
            <div>Which of your friends are attending? - <ul v-for="attendee in event.attendees" :key="attendee"><li>{{attendee}}</li> </ul> </div>
            <div class="row">
                <button type="button" v-on:click="edit(event)">Edit</button>
                <button v-on:click="remove(event)" type='button'>Delete</button>
            </div>
            <div :id="event.date+event._id" hidden>
                    <form v-on:submit.prevent="submitEdit(event)">
                    <div class="row"><input required type='text' placeholder="New Title" v-model='title'></div>
                    <div class='row'><input required type='text' v-model='description' placeholder="New Description/About Info"></div>
                        <div class='row'>New Date - <input required type='date' v-model='date'></div>
                        <div class='row'><input required type='text' v-model='location' placeholder="New Location"></div>
                        <div class="row"><button :name="event._id" type="submit" >Submit Changes</button>
                    </div>
                    </form>
                    </div>
            <div class="row">
                <form>
                    <input type="text" name="fulL_name" v-model="full_name" placeholder="Full Name" required>
                    <button class="attendButton" v-on:click="willAttend(event)" type="button">Press to RSVP</button>
                </form>
            </div>
        </div>
    </div>
</div>
</template>

<script>
import axios from 'axios';
    
export default {
  name: "Board",
    data() {
        return {events: this.events};
    },
    created() {
        this.getEvents();
    },
        methods: {
            async getEvents() {
                try {
                    let res = await axios.get("/api/events");
                    this.events = res.data;
                    return true;
                } catch (error) {
                    console.log(error);
                }
            },
            async willAttend(event) {
                try {
                    const fullname = this.full_name;
                    await axios.put("/api/events/attendee/" + event._id, {
                        new_attendee: 1,
                        new_attendee_name: fullname
                    });
                    this.getEvents();
                    return true;
                } catch (error) {
                    console.log(error);
                }
            },
            edit(event) {
                document.getElementById(event.date + event._id).hidden = false;
            },
            async submitEdit(event) {
                console.log("in submitEdit()");
                try {
                    await axios.put("/api/events/" + event._id, {
                        title: this.title,
                        description: this.description,
                        location: this.location,
                        date: this.date
                    });
                    this.getEvents();
                    return true;
                } catch (error) {
                    console.log(error);
                }
            },
            async remove(event) {
                try {
                    await axios.delete("/api/events/" + event._id);
                    this.getEvents();
                    return true;
                } catch(error) {
                    console.log(error);
                }
            }
        }
};
</script>

<style scoped>
    .announcement {
        border: 4px red dashed;
        margin: 1em;
        background-color: lightgrey;
    }
</style>