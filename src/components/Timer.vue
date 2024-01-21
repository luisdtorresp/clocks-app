
<template>
    <div class="timer">
        <h3>{{ this.title }}</h3>
        <h1 id="countdown">{{ formatTime }}</h1>
        <div class="buttons">
            <button @click="startTimer" :disabled="isTimerRunning">Start</button>
            <button @click="stopTimer" :disabled="!isTimerRunning">Stop</button>
            <button @click="resetTimer">Reset</button>
        </div>
        <div class="settings">
            
            <q-toggle
                v-model="this.autoStart"
                icon="alarm"
                label="Auto Start"
            />
        </div>
        
    </div>
</template>
    
<script>
import alarm from '../assets/alarm-clock-beep.mp3';
export default {
    props: {
        hours: {
            type: Number,
            default: 0
        },
        minutes: {
            type: Number,
            default: 0
        },
        seconds: {
            type: Number,
            default: 0
        },
        title: {
            type: String,
            default: 'Timer'
        },
        description: {
            type: String,
            default: 'A simple timer component'
        },
        autoStart: {
            type: Boolean,
            default: false
        }
    },
    data() {
        return {
            timer: null,            
            time: 0, // Seconds
            isTimerRunning: false,
            
            
        };
    },
    computed: {
        formatTime() {
            const hours = Math.floor(this.time / 3600);
            const minutes = Math.floor((this.time % 3600) / 60);
            const seconds = this.time % 60;
            return `${hours.toString().padStart(2, '0')}h:${minutes.toString().padStart(2, '0')}m:${seconds.toString().padStart(2, '0')}s`;
        }
    },
    methods: {
        startTimer() {
            if (!this.isTimerRunning) {
                if (this.time === 0) {
                    this.time = this.hours * 3600 + this.minutes * 60 + this.seconds * 1;
                    console.log(this.time);
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
                        const msg = this.title ? this.title+". Time's up!" : "Timer complete!";
                        this.notification(msg);

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
            this.time = this.hours * 3600 + this.minutes * 60 + this.seconds * 1;
            this.isTimerRunning = false; 
            
        },
        notification(msg = "Time is Up!") {
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
    },
    mounted() {
        this.time = this.hours * 3600 + this.minutes * 60 + this.seconds * 1;
        if (this.autoStart) {
            this.startTimer();
        }
    }
   
};
</script>

<style scoped>
.timer {
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

