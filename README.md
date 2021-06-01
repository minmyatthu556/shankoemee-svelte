# Shan Koe Mee

### This svelte project is hosted at https://shankoemee-svelte.vercel.app ###

## How to play
1. Both user and bot are first given two cards each. 
2. Find the sum of the numbers of your cards. A represents 1 and K, Q, J represent 10. Take the last digit of the sum (i.e if the sum is 19, your score is 9, etc). That's your number score. 
3. Your suit score is the suit of the card with largest number. Even though A has the value of 1, it's regarded as the largest value in the deck. It goes like A > K > Q > J > 9 > ... > 2. So if you have A and K, the suit of A is your suit score. 
4. You will have an option to draw one card. If you're satisfied with your score with just two cards, you don't need to draw.
5. If you draw, find the score of your numbers and suits using the above method.
6. Bot will do the same. 

### How the game works
You will need to compare your number score to the bot's number score. The person with larger number score wins. 
If both parties have the same number score, the one with less card wins.
If you both have the same number of cards and the number scores are the same, you'll need to compare the suit score. 
The suit score works like this: Spade > Diamond > Heart > Clubs.
The person with the larger suit score wins.
If both have the same suit scores and the same number scores, the person with less card win. 
