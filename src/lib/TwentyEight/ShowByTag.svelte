<script>
import { dataset_dev } from 'svelte/internal';

    import TwentyEight from './TwentyEight.svelte';

    export let tag, db, data;

</script>

<section id="parentContainer">

<h3>{tag}</h3>

    {#if data}

    {#each Object.keys(data) as key, index}
    <!-- <TwentyEight db={db} entireCollection={data} key={key}  /> -->
  
  
    <!-- {data[key].days[DateTime.now().toISODate()].isCompleted} -->
  
    <!-- {data[key].tags} -->
  
    <!-- {#if data[key].tags && data[key].tags.includes('Alaska Airlines')} -->
    {#if data[key].tags && data[key].tags.includes(tag)}
    <TwentyEight db={db} entireCollection={data} key={key}  />
    {/if}
    
    {/each}
  
    {/if}

</section>
<style>
    h3 {
        margin: 0;
    }
    #parentContainer { 
        /* border: 1px dotted red; */

        padding: 0;
        margin: 0 0 40px 0; 
        }
</style>