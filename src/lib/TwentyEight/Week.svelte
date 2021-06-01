<script>
	import Day from './Day.svelte';
	import { DateTime } from 'luxon';

	const timesToBeRendered = 7;
	export let docRef, data, firstDayDateTime, weekIndex;
	// console.log('Week firstDayDateTime', firstDayDateTime);
	// console.log('Week data', data);
</script>

<section>
	<!-- {dateTime} -->
    <!-- <br/> -->
	



	{#each Array(timesToBeRendered) as _, index}
		<!-- <Day dateTime={firstDayDateTime + index + (7 * weekIndex)} /> -->
<Day docRef={docRef} data={data} dateTime={firstDayDateTime} {weekIndex} dayIndex={index} />
    {/each}
    
    
</section>

<style>
	section {
		display: inline-grid;
		grid-template-columns: 1fr 1fr 1fr 1fr 1fr 1fr 1fr;
	}


</style>
