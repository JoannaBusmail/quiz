<template>
    <header>
        <h4>Question {{ questionNumber }}
        </h4>
        <div class="bar">
            <div
                class="completion"
                :style="{ width: `${progressBar}%` }"
            ></div>
        </div>
    </header>
</template>

<script setup>

const props = defineProps({
    questionNumber: String,
    progressBar: Number
})
</script>

<style scoped>
header {
    margin-top: 20px;
}

header h4 {
    font-size: 30px;

}

.bar {
    width: 300px;
    height: 20px;
    border: 3px solid bisque;
}

.completion {
    height: 100%;
    background-color: bisque;
}
</style>