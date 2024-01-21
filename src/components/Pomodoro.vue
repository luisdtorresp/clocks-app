<template>
    <div class="pomodoro">
        <div class="timer-type">
            <label>
                <input type="radio" v-model="timerType" @change="resetTimer" value="workTime">
                Work Time
            </label>
            <label>
                <input type="radio" v-model="timerType" @change="resetTimer" value="breakTime">
                Break Time
            </label>
        </div>
        <h1 id="countdown">{{ formatTime }}</h1>
        <div class="buttons">
            <button @click="startTimer" :disabled="isTimerRunning">Start</button>
            <button @click="stopTimer" :disabled="!isTimerRunning">Stop</button>
            <button @click="resetTimer">Reset</button>
        </div>
        <div class="settings">
            <label for="work-time">Work Time:</label>
            <input id="work-time" type="number" v-model="workTime" min="1" max="60">
            <label for="break-time">Break Time:</label>
            <input id="break-time" type="number" v-model="breakTime" min="1" max="60">
            <q-toggle
                v-model="autoStart"
                icon="alarm"
                label="Auto Start"
            />
        </div>
        
    </div>
</template>
    
<script>
import alarm from '../assets/alarm-clock-beep.mp3';
export default {
    data() {
        return {
            timer: null,
            workTime: 25, //minutes
            breakTime: 5, //minutes
            time: 0, // Seconds
            autoStart: false,
            isTimerRunning: false,
            timerType: 'workTime'
        };
    },
    computed: {
        formatTime() {
            const minutes = Math.floor(this.time / 60);
            const seconds = this.time % 60;
            return `${minutes.toString().padStart(2, '0')}:${seconds.toString().padStart(2, '0')}`;
        }
    },
    methods: {
        startTimer() {
            if (!this.isTimerRunning) {
                if (this.time === 0) {
                    this.time = this.timerType === 'workTime' ? this.workTime * 60 : this.breakTime * 60;
                }
                this.timer = setInterval(() => {
                    if (this.time > 0) {
                        this.time--;
                    } else {
                        clearInterval(this.timer);
                        this.isTimerRunning = false;
                        // Timer finished, do something here
                        this.soundAlert();

                        // Customize notification message on timer completion via argument
                        const msg = this.timerType === 'workTime' ? "Work interval complete!" : "Break interval complete!";
                        this.notification(msg);

                        

                        if (this.autoStart) {
                            this.timerType = this.timerType === 'workTime' ? 'breakTime' : 'workTime';
                            this.startTimer();
                        }
                    }
                }, 1000);
                this.isTimerRunning = true;
            }
        },
        stopTimer() {
            if (this.isTimerRunning) {
                clearInterval(this.timer);
                this.isTimerRunning = false;
            }
        },
        resetTimer() {
            clearInterval(this.timer);
            this.time = 0;
            this.isTimerRunning = false;

            // Change countdown inner text to work time or break time value depending on timerType
            if (this.timerType === 'workTime') {
                document.getElementById('countdown').innerText = this.workTime.toString().padStart(2, '0') + ':00';
            } else {
                document.getElementById('countdown').innerText = this.breakTime.toString().padStart(2, '0') + ':00';
            }
            
        },
        notification(msg = "Session complete!") {
            if (!("Notification" in window)) {
                // Check if the browser supports notifications
                alert("This browser does not support desktop notification");
            } else if (Notification.permission === "granted") {
                // Check whether notification permissions have already been granted;
                // if so, create a notification
                const notification = new Notification(msg);
                // …
            } else if (Notification.permission !== "denied") {
                // We need to ask the user for permission
                Notification.requestPermission().then((permission) => {
                    // If the user accepts, let's create a notification
                    if (permission === "granted") {
                        const notification = new Notification(msg);
                        // …
                    }
                });
            }
        },
        soundAlert() {
            const audio = new Audio(alarm);
            audio.play();
        }
    }
};
</script>

<style scoped>
.pomodoro {
    text-align: center;
}

.buttons {
    margin-top: 20px;
}

button {
    margin: 0 5px;
}

.settings {
    margin-top: 20px;
    padding: 2rem;
    border-radius: 1rem;
    border: solid 1px #ccc;
}
</style>

