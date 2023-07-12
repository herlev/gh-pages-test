<script lang="ts">
  import "./app.css"
  import seedrandom from "seedrandom"
  import ListItem from "./ListItem.svelte"
  let workout_day: string
  let exercises: [string] | []
  let rng: seedrandom

  function getRandomAndRemove(list) {
    let i = Math.floor(rng()*list.length)
    return list.splice(i, 1)[0]
  }
  import all_exercises from "./exercises.json"
  function selectExercises(day) {
    let d = new Date()
    let seed = `${d.getFullYear()}/${d.getMonth()}/${d.getDate()}`
    rng = seedrandom(seed)
    let e = all_exercises[day]
    let ee = e["compound"].concat(e["accessory"])
    exercises = []
    for (let i=0; i<6; i++) {
      exercises.push(getRandomAndRemove(ee))
    }
    workout_day = day
  }
  // selectExercises("push")
</script>

<main class="p-8 max-w-xl mx-auto flex flex-col">
{#if workout_day}
  <button on:click={() => workout_day = null}><img src="back.svg" alt="back button"/></button>
  <div class="flex flex-col flex-1">
    <div class="text-2xl flex-1 flex flex-col justify-center"><div>Today's <span class="text-primary">{workout_day}</span> workout</div></div>
    <ol class="flex flex-col gap-2">
    {#each exercises as e, i}
      <ListItem i={i+1} text={e}/>
    {/each}
    </ol>
  </div>
{:else}
  <div class="text-xl text-primary text-center mt-48">What are you training today?</div>
  <div class="flex space-around gap-4 mt-20">
    {#each ["push", "pull", "legs"] as day}
    <button class="gray capitalize text-sm p-2 rounded-lg flex-1" on:click={() => selectExercises(day)}>{day}</button>
    {/each}
  </div>
{/if}
</main>

<style>

:global(body) {
  background: #1b1a1f;
  color: #e6e2e4;
  font-family: sans-serif;
}

main {
  height: 100dvh;
}

.gray {
  background: #302e38;
}

</style>
