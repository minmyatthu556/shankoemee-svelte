<script lang="ts">
  import Card from './components/Card.svelte'
  import Header from './components/Header.svelte'
  import { buildADeck, turnKQJto10, printCard } from './utils/helpers'
  import { findBiggerSuit, findNumberScore, gameLogic } from './utils/logics'

  const suits = ['♣️', '♦️', '♥️', '♠️']
  const numbers = ['2', '3', '4', '5', '6', '7', '8', '9', 'J', 'Q', 'K', 'A']

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

  let buttonDisable = false
  let showBot = false
  let showWinner = false

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

  let winner = ''

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
      <button
        class="py-3 px-5 font-semibold bg-gray-900 text-gray-50 uppercase tracking-wide rounded-full transform transition hover:bg-gray-800 hover:-translate-y-0.5 hover:shadow-none shadow-xl focus:outline-none
			focus:ring-1 focus:ring-offset-gray-900 mx-5 my-2"
        on:click={handleDraw}
        disabled={buttonDisable}>Draw A Card</button
      >
      <button
        class="py-3 px-5 font-semibold bg-gray-900 text-gray-50 uppercase tracking-wide rounded-full transform transition hover:bg-gray-800 hover:-translate-y-0.5 hover:shadow-none shadow-xl focus:outline-none
			focus:ring-1 focus:ring-offset-gray-900 mx-5 my-2"
        on:click={handleDontDraw}
        disabled={buttonDisable}>I won't draw</button
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
<footer class="mt-auto w-full mx-auto text-sm text-gray-600 text-center py-2 ">
  By Min Myat Thu
</footer>

<style global lang="postcss">
  @tailwind base;
  @tailwind components;
  @tailwind utilities;
</style>
