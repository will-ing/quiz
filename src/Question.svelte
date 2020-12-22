<script>
    import { score } from "./store.js";
    export let quest;
    export let nextQuestion;

    let isCorrect;
    let isAnswered = false;

    let answers = quest.incorrect_answers.map((answer) => {
        return {
            answer,
            correct: false,
        };
    });

    let allAnswers = [
        ...answers,
        {
            answer: quest.correct_answer,
            correct: true,
        },
    ];

    const shuffle = (arr) => {
        return arr.sort(() => Math.random() - 0.5);
    };

    const checkQuestion = (check) => {
        // if (!isAnswered) {
        isAnswered = true;
        isCorrect = check;
        if (check) {
            score.update((val) => val + 1);
        }
        // }
    };

    shuffle(allAnswers);
</script>

<style>
    h5.isCorrect {
        color: green;
    }
    h5.wrong {
        color: red;
    }
</style>

<h3>
    {@html quest.question}
</h3>
{#if isAnswered}
    <h5 class:isCorrect class:wrong={!isCorrect}>
        {#if isCorrect}You got it right{:else}Wrong!{/if}
    </h5>
{/if}

{#each allAnswers as ans}
    <button disabled={isAnswered} on:click={() => checkQuestion(ans.correct)}>
        {@html ans.answer}
    </button>
{/each}

{#if isAnswered}
    <div><button on:click={nextQuestion}>Next Question</button></div>
{/if}
