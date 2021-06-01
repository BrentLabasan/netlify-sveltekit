<script>
import { dataset_dev } from 'svelte/internal';
    import { DateTime } from 'luxon';

    import TwentyEight from './TwentyEight.svelte';

    export let tag, db, data;

    // 28s that are uncompleted for today are displayed before 28s that have been completed

    let newData = [], unsortedComplete = [], unsortedIncomplete = [];

    data = Object.entries(data).sort((a, b) => { 
        return a[1].name > b[1].name ? 1 : -1;
    });

    data.map((x) => {
        if (x[1].days[DateTime.now().toISODate()].isCompleted) {
            unsortedComplete.push(x);
} else {
            unsortedIncomplete.push(x);

        }
    });


    // Object.entries(data).sort((a, b) => {
    //             debugger;

    //     const boolah = a[1][1].days[DateTime.now().toISODate()].isCompleted && !b[1][1].days[DateTime.now().toISODate()].isCompleted;

    //     console.log("boolah", boolah);


    //     // return boolah ? 1 : -1;

    //     if (boolah) {
    //         unsortedIncomplete.push(a);
    //     } else {
    //         unsortedComplete.push(a);
    //     }
    // });

    // unsortedComplete = unsortedComplete.sort((a, b) => {
    //     const taco = a[1].name > b[1].name ? 1 : -1;
    //     // console.log("taco", a[1].name, b[1].name,  taco);
    //     return taco;
    // });

    // unsortedIncomplete = unsortedIncomplete.sort((a, b) => {
    //     return a[1].name > b[1].name ? 1 : -1;
    // });

    

    newData = unsortedIncomplete.concat(unsortedComplete);
    
    // newData = data;
    
    // console.log("ShowSelected", tag, data);
    // console.log("ShowSelected", tag, newData);
</script>

<section id="parentContainer">

<h3>{tag}</h3>

    {#if newData}

    {#each newData as twentyEight, index}
    <!-- <TwentyEight db={db} entireCollection={data} key={key}  /> -->
  
  
    <!-- {data[key].days[DateTime.now().toISODate()].isCompleted} -->
  
    <!-- {data[key].tags} -->
  
    <!-- {#if data[key].tags && data[key].tags.includes('Alaska Airlines')} -->
    <TwentyEight db={db} entireCollection={data} key={twentyEight[1].id}  />
    
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