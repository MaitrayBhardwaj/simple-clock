<script>
	import { onMount, onDestroy } from 'svelte';

	const zones = ['IST', 'UTC', 'JST', 'CET']
	let currZone = 0
	const options = { weekday: 'long', year: 'numeric', month: 'long', day: 'numeric' };

	let timerID, time = new Date().toLocaleTimeString('en-US', { timeZone: zones[currZone] });

	let date = new Date().toLocaleDateString('en-US', { weekday: 'long', year: 'numeric', month: 'long', day: 'numeric', timeZone: zones[currZone] })

	const tick = () => {
		time = new Date().toLocaleTimeString('en-US', { timeZone: zones[currZone] })
		date = new Date().toLocaleDateString('en-US', { weekday: 'long', year: 'numeric', month: 'long', day: 'numeric', timeZone: zones[currZone] })
	}

	const backgrounds = [
		'https://wallpaperaccess.com/full/1896134.jpg',
		'https://wallpaperaccess.com/full/4688678.jpg',
		'https://wallpaperaccess.com/full/2346347.jpg',
		'https://wallpaperaccess.com/full/236182.jpg'
	]

	onMount(() => {
		timerID = setInterval(tick, 1000);
		document.body.style.backgroundImage = `url(${backgrounds[currZone]})`
		document.body.style.backgroundPosition = `center`
		document.body.style.backgroundRepeat = `no-repeat`
		document.body.style.backgroundSize = `cover`
		document.body.style.padding = `0`
	})

	onDestroy(() => {
		clearInterval(timerID)
	})

	const changeZone = (zone) => {
		currZone = zone
		document.body.style.backgroundImage = `url(${backgrounds[currZone]})`
		tick()
	}

</script>

<main>
	<div class="time">
		<span class="hhmmss">
			{ time.length === 10 ? `0${time.substring(0, 7)}` : `${time.substring(0, 8)}` }
		</span>
		<span class="meridian">
			{ time.substring(8, 11) }
		</span>
	</div>

	<div class="date">
		{ date }
	</div>

	<div class="zones">
		{#each zones as zone, i}
			<button class:selected = { i === currZone } on:click = { () => changeZone(i) }>{ zone }</button>
		{/each}
	</div>
</main>

<style>
	main{
		display: flex;
		justify-content: center;
		align-items: center;
		flex-direction: column;
		justify-self: flex-end;
		align-self: flex-start;
	}

	.date{
		color: white;
		font-size: 1.5em;
		margin-bottom: 2em;
	}
	
	.time{
		font-size: 5em;
		text-align: center;
		margin: 1em;
		color: white;
	}

	.meridian{
		font-size: 2rem;
		color: white;
	}

	.zones button{
		margin: 0 5px;
		padding: 0.5em 1em;
		color: white;
		background-color: transparent;
		border: none;
		border-bottom: 2px solid transparent;
		transition: border-bottom-color 0.2s;
	}

	.zones button:hover, .zones button:active{
		border-bottom-color: white;
	}

</style>