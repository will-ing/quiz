<script>
    import { fade, blur, fly, slide } from "svelte/transition";
    import { onMount } from "svelte";
    import Question from "./Question.svelte";
    import Modal from "./Modal.svelte";
    import { score } from "./store.js";
    let isModalOpen = false;
    let activeQuestion = 0;

    let quiz = getQuiz();

    // onMount(() => {

    // });

    async function getQuiz() {
        const res = await fetch(
            "https://opentdb.com/api.php?amount=10&category=12&type=multiple"
        );
        const quiz = await res.json();
        return quiz;
    }

    function nextQuestion() {
        activeQuestion = activeQuestion + 1;
    }

    function resetQuiz() {
        isModalOpen = false;
        score.set(0);
        activeQuestion = 0;
        quiz = getQuiz();
    }

    // marks any statement as reactive in svelte
    // Reactive statement
    $: if ($score > 1) {
        isModalOpen = true;
    }
</script>

<style>
    .fade-wrapper {
        position: fixed;
    }
</style>

<div>
    <button on:click={resetQuiz}>Get New Quiz</button>

    <h3>My score: {$score}</h3>
    <h4>Question #{activeQuestion + 1}</h4>

    {#await quiz}
        Loading.....
    {:then data}
        {#each data.results as quest, i}
            {#if i === activeQuestion}
                <div
                    in:fly={{ x: 100 }}
                    out:fly={{ x: -200 }}
                    class="fade-wrapper">
                    <Question {nextQuestion} {quest} />
                </div>
            {/if}
        {/each}
    {/await}
</div>

<!-- https://opentdb.com/api.php?amount=10&category=12&type=multiple -->

{#if isModalOpen}
    <Modal on:close={resetQuiz}>
        <h2>You Won!</h2>
        <p>Congrats!</p>
        <button on:click={resetQuiz}>Start over</button>
    </Modal>
{/if}
