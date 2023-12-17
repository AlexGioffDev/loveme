<script lang="ts">
    import {ref, reactive} from 'vue';

    export default {
        setup() {
            let text = ref("Do You Love me?");
            let isClicked = ref(false);
            const noButton = ref(null);
            const hearts = reactive([]);
            const changeText = (newText: string) => {
                text.value = newText;
                isClicked.value = true;
            }
            const moveButton = () => {
                if (noButton.value) {
                    const rect = noButton.value.getBoundingClientRect();
                    const randomX = Math.random() * (window.innerWidth - rect.width);
                    const randomY = Math.random() * (window.innerHeight - rect.height);
                    noButton.value.style.position = 'absolute';
                    noButton.value.style.left = randomX + 'px';
                    noButton.value.style.top = randomY + 'px';
                }
            }

            const makeRain = () => {
                for (let i = 0; i < 100; i++) {
                    setTimeout(() => {
                        hearts.push({
                            id: Math.random(),
                            left: Math.random() * window.innerWidth + 'px',
                            top: Math.random() * window.innerHeight + 'px'
                        });
                    }, i * 100);
                }
            }

            return {text, changeText, isClicked, noButton, moveButton, hearts, makeRain}
        }
    }
</script>

<template>
    <div class="container">
        <h1>{{text}}</h1>
        <img src="/Teddy.png" />
        <div class="wrapper-buttons">
            <button :disabled="isClicked" @click="changeText('Yes!!!!'); makeRain()">
                Yes
            </button>
            <button :disabled="isClicked" @mouseover="moveButton" ref="noButton">
                No
            </button>
        </div>
        <div v-for="heart in hearts" :key="heart.id" :style="{left: heart.left, top: heart.top}" class="heart">
            ❤️
        </div>
    </div>
</template>


<style>
    h1{
        text-transform: uppercase;
        font-size: 3.5rem;
        color: transparent;
        -webkit-text-stroke: 3px white;
    }

    .container {
        width: 100%;
        height: 80rem;
        padding: 2rem;
        display: flex;
        flex-direction: column;
        align-items: center;
        justify-content: center;
        gap: 2rem;
    }

    .container img {
        width: 10rem;
        height: 10rem;
    }

    .wrapper-buttons {
        display: flex;
        gap: 2rem;
        justify-content: center;
        align-items: center;
    }

    .wrapper-buttons button {
        padding:0.6rem 2rem;
        font-size: 2.5rem;
        background-color: rgba(238,174,202, 0.8);
        border: 1px solid white;
        border-radius: 50px;
        color: white;
        text-transform: uppercase;
        font-weight: bold;
    }

    .wrapper-buttons button:disabled {
        opacity: 0.6;
    }

    .heart {
        position: absolute;
        font-size: 2rem;
        animation: fall 4s linear infinite;
    }

    @keyframes fall {
        0% { transform: translateY(-100%); }
        100% { transform: translateY(100%); }
    }
</style>