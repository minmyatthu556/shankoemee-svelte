<script lang="ts">
  export let card: string[]
</script>

<div
  class="border w-20 h-28 border-gray-900 rounded-lg m-auto mt-5 bg-gray-100 shadow-lg"
>
  <div class="text-5xl pl-2 pt-2">{card[0]}</div>
  <div class="text-4xl text-right pr-2">{card[1]}</div>
</div>
