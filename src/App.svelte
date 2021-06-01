<script lang="ts">
  import Card from './components/Card.svelte'
  import Header from './components/Header.svelte'
  import { buildADeck } from './utils/helpers'
  import { findBiggerSuit, findNumberScore, gameLogic } from './utils/logics'

  let buttonDisable = false
  let showBot = false
  let showWinner = false

  let deck = buildADeck()

  const drawACard = (): string[] => {
    const randomIndex = Math.floor(Math.random() * deck.length)
    const chosenCard = deck[randomIndex]

    deck = deck.filter((card) => card !== chosenCard)
    return chosenCard
  }

  const userFirstCard = drawACard()
  const botFirstCard = drawACard()
  const userSecondCard = drawACard()
  const botSecondCard = drawACard()

  let botDrawCard: string[] | undefined
  let userDrawCard: string[] | undefined

  let userSuitScore = findBiggerSuit(userFirstCard, userSecondCard)
  let botSuitScore = findBiggerSuit(botFirstCard, botSecondCard)

  let userNumScore = findNumberScore(userFirstCard, userSecondCard)
  let botNumScore = findNumberScore(botFirstCard, botSecondCard)

  if (botNumScore < 5) {
    botDrawCard = drawACard()
    botSuitScore = findBiggerSuit(botFirstCard, botSecondCard, botDrawCard)
    botNumScore = findNumberScore(botFirstCard, botSecondCard, botDrawCard)
  }

  let winner = ''

  const handleDraw = () => {
    userDrawCard = drawACard()
    buttonDisable = true
    showBot = true
    userSuitScore = findBiggerSuit(userFirstCard, userSecondCard, userDrawCard)
    userNumScore = findNumberScore(userFirstCard, userSecondCard, userDrawCard)
    const gameCards = {
      userNum: Number(userNumScore),
      userSuit: userSuitScore,
      botNum: Number(botNumScore),
      botSuit: botSuitScore,
      userFirst: userFirstCard,
      userSecond: userSecondCard,
      userThird: userDrawCard,
      botFirst: botFirstCard,
      botSecond: botSecondCard,
      botThird: botDrawCard,
    }

    winner = gameLogic(gameCards)

    showWinner = true
  }

  const handleDontDraw = () => {
    buttonDisable = true
    showBot = true
    const gameCards = {
      userNum: Number(userNumScore),
      userSuit: userSuitScore,
      botNum: Number(botNumScore),
      botSuit: botSuitScore,
      userFirst: userFirstCard,
      userSecond: userSecondCard,
      userThird: userDrawCard,
      botFirst: botFirstCard,
      botSecond: botSecondCard,
      botThird: botDrawCard,
    }

    winner = gameLogic(gameCards)

    showWinner = true
  }
</script>

<Header />
<main>
  <div class="w-4/5 m-auto">
    <h1 class="my-5 text-3xl text-center font-semibold">User Cards</h1>
    <div class="flex items-center justify-center">
      <Card card={userFirstCard} />
      <Card card={userSecondCard} />
      {#if userDrawCard}
        <Card card={userDrawCard} />
      {/if}
    </div>
    <div class="w-full text-center mx-auto my-10">
      <button class="btn" on:click={handleDraw} disabled={buttonDisable}
        >Draw A Card</button
      >
      <button class="btn" on:click={handleDontDraw} disabled={buttonDisable}
        >I won't draw</button
      >
    </div>

    {#if showBot}
      <h1 class="my-5 text-3xl text-center font-semibold">Bot Cards</h1>
      <div class="flex items-center justify-center">
        <Card card={botFirstCard} />
        <Card card={botSecondCard} />
        {#if botDrawCard}
          <Card card={botDrawCard} />
        {/if}
      </div>
    {/if}
  </div>
  {#if showWinner}
    <div class="flex justify-center items-center my-8">
      <h1 class="text-center text-3xl font-bold py-5">
        Winner: <span class="font-semibold uppercase tracking-wide"
          >{winner}</span
        >
      </h1>
    </div>
  {/if}
</main>
<footer class="mt-auto w-full mx-auto text-sm text-gray-500 text-center py-2 ">
  By Min Myat Thu
  <br />
  <a
    rel="noopener"
    href="https://github.com/minmyatthu556/shankoemee-svelte"
    target="_blank">You can read the game rules at this link</a
  >
</footer>

<style global lang="postcss">
  @tailwind base;
  @tailwind components;
  @tailwind utilities;

  @layer components {
    .btn {
      @apply py-3 px-5 font-semibold bg-gray-800 text-gray-50 uppercase 
			tracking-wide rounded-full transform transition hover:bg-gray-600 hover:-translate-y-0.5 
			shadow-xl focus:outline-none
			focus:ring-gray-800 focus:ring focus:ring-offset-1 mx-5 my-2;
    }
  }
</style>
