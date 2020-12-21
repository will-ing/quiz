<script>
    export let quest;

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
        isAnswered = true;
        isCorrect = check;
    };

    shuffle(allAnswers);
</script>

<h3>
    {@html quest.question}
</h3>
{#if isAnswered}
    <h4>
        {#if isCorrect}You got it right{:else}You goofed up{/if}
    </h4>
{/if}

{#each allAnswers as ans}
    <button on:click={() => checkQuestion(ans.correct)}>
        {@html ans.answer}
    </button>
{/each}
