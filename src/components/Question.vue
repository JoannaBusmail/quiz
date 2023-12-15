<template>
    <div class="question-container">
        <h1 class="question"> {{ question.text }}</h1>

    </div>
    <div class="options-container">
        <!--PROP-->
        <!--question is the PROP with the info sent by the parent-->
        <!-- question is an object and we can iterate in it-->
        <!-- EMIT-->
        <!--is here in the child where we have the options and where we know if is correct or not-->
        <!--we have to send thisinfo to the parent-->
        <div
            v-for="option in question.options"
            :key="option.id"
            class="option"
            @click="emitSelectedOption(option.isCorrect)"
        >
            <p class="option-label">{{ option.label }}</p>
            <div class="option-value">
                <p>{{ option.text }}</p>
            </div>

        </div>
        <p>Click in the option or press the corresponding keyboard letter</p>
    </div>
</template>

<script setup>

const props = defineProps({
    question: {
        type: Object,
        required: true
    }
})

// need emit to pass the correct answer to the parent
// params: name of the event and the value
const emit = defineEmits([ 'selectOption' ])

const emitSelectedOption = (isCorrect) =>
{ emit("selectOption", isCorrect) }



</script>

<style scoped>
.question-container {
    margin-top: 20px;
}

.question {
    font-size: 40px;
    margin-bottom: 20px;
}

.option {
    display: flex;
    margin-bottom: 20px;
    cursor: pointer
}

.option-label {
    background-color: rgb(196, 200, 255);
    width: 50px;
    height: 50px;
    border-radius: 100%;
    font-size: 30px;
    display: flex;
    align-items: center;
    justify-content: center;
    margin-right: 10px;

}

.option-value {
    background-color: rgb(244, 239, 239);
    width: 100%;
    height: 50px;
    font-size: 30px;
    padding: 0 20px;
    border-radius: 10px;
}
</style>
```