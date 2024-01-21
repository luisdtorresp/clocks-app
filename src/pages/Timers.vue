// Note: This is the main page for the Timers section of the app. It will be the page that is displayed when the user clicks on the Timers icon in the bottom navigation bar. It will display a list of timers that the user has created. The user will be able to add a new timer by clicking on the floating action button (FAB) in the bottom right corner. The user will also be able to edit the settings for each timer by clicking on the timer in the list. The user will be able to start, pause, and reset each timer. The user will also be able to delete a timer by clicking on the delete icon in the top right corner of the timer card.

<template>
    <q-page class="flex row flex-center">
        <q-card v-for="timer in timers" :key="timer.id">
            <q-card-section>
                <q-item>
                    <q-item-section>
                        <Timer
                            :title="timer.title" 
                            :hours="timer.hours"
                            :minutes="timer.minutes"
                            :seconds="timer.seconds" 
                            :auto-start="timer.autoStart"
                        />
                    </q-item-section>
                    <q-item-section side>
                        <q-icon name="delete" class="cursor-pointer" @click="deleteTimer(timer.id)" />
                    </q-item-section>
                </q-item>
            </q-card-section>
        </q-card>
        
        <div class="settings">
            <q-input
            v-model="title"
            label="Title"
            dense
            />

            <q-input
                v-model="hours"
                type="number"
                label="Hours"
                min="0"
                max="99"
                step="1"
                dense
            />
            <q-input
                v-model="minutes"
                type="number"
                label="Minutes"
                min="0"
                max="59"
                step="1"
                dense
            />
            <q-input
                v-model="seconds"
                type="number"
                label="Seconds"
                min="0"
                max="59"
                step="1"
                dense
            />
            <q-toggle
                v-model="autoStart"
                icon="alarm"
                label="Auto Start"
            />
        </div>
        <q-btn fab
            icon="add"
            color="primary"
            class="fab"
            @click="addTimer"
        />
    </q-page>
</template>
<script setup>
    import { ref } from 'vue';
    import Timer from '../components/Timer.vue';
    const timers = ref([]);
    const title = ref('');
    const hours = ref(0);
    const minutes = ref(0);
    const seconds = ref(0);
    const autoStart = ref(false);

    const addTimer = () => {
        // Add logic to create a new timer
        timers.value.push({
            id: timers.value.length,
            title: title.value,
            hours: hours.value,
            minutes: minutes.value,
            seconds: seconds.value,
            autoStart: autoStart.value
        });
    };

    const deleteTimer = (id) => {
        // Add logic to delete the timer with the given id
        clearInterval(timers.value[id].timer);
        timers.value = timers.value.filter(timer => timer.id !== id);
    };
</script>
<style></style>
