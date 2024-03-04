<script setup>
import { ref, onUnmounted } from 'vue';

const images = [
    {
        src: 'https://petapixel.com/assets/uploads/2022/12/what-is-unsplash-800x420.jpg',
        alt: 'Image 1',
    },
    {
        src: 'https://petapixel.com/assets/uploads/2022/12/image13-1-800x536.jpg',
        alt: 'Image 2',
    },
    {
        src: 'https://petapixel.com/assets/uploads/2022/12/image11-1-800x534.jpg',
        alt: 'Image 3',
    },
];

const currentSliderIndex = ref(0);
let intervalId;

const isTimerPaused = ref(false);

const nextSlide = () => {
    currentSliderIndex.value = (currentSliderIndex.value + 1) % images.length;
    isTimerPaused ? '' : startSlider()
};

const prevSlide = () => {
    currentSliderIndex.value = (currentSliderIndex.value - 1 + images.length) % images.length;
    isTimerPaused ? '' : startSlider()
};

const startSlider = () => {
    intervalId = setInterval(() => {
        nextSlide();
    }, 3000);
};

const playSlider = () => {
    isTimerPaused.value = false;
    startSlider();
}

const stopSlider = () => {
    clearInterval(intervalId);
    isTimerPaused.value = true;
}

startSlider();

onUnmounted(() => {
    clearInterval(intervalId);
});
</script>

<template>
    <div class="h-screen flex flex-col">
        <div class="bg-slate-500 text-center p-4 uppercase text-gray-50">
            Image Slider
        </div>

        <div class="slider flex h-[100%]">
            <div class="flex mx-auto justify-center relative w-[500px] h-[300px] m-auto">
                <template v-for="(image, index) in images" :key="index">
                    <transition name="fade">
                        <img :src="image.src" :alt="image.alt" class="aspect-[16/9] absolute"
                            v-if="index === currentSliderIndex" />
                    </transition>
                </template>

                <!-- next and previous buttons -->
                <i class="fas fa-caret-right absolute right-0 top-1/3 text-4xl m-2 cursor-pointer text-white z-50"
                    @click="nextSlide"></i>
                <i class="fas fa-caret-left absolute left-0 top-1/3 text-4xl m-2 cursor-pointer text-white z-50"
                    @click="prevSlide"></i>

                <!-- play or pause buttons -->
                <i class="fa-solid fa-circle-play absolute bottom-5 text-3xl m-2 cursor-pointer text-gray-500 z-50"
                    @click="playSlider" v-if="isTimerPaused"></i>
                <i class="fa-solid fa-pause absolute bottom-5 text-3xl  m-2 cursor-pointer shadow-2xl text-gray-500 z-50"
                    @click="stopSlider" v-else="isTimerPaused"></i>
            </div>
        </div>
    </div>

</template>

<style>
.fade-enter-active,
.fade-leave-active {
    transition: opacity 1s, transform 1s;
}

.fade-enter-from,
.fade-leave-to {
    opacity: 0;
    transform: translateX(0);
}
</style>
