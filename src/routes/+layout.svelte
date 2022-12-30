<script>
	import { onMount } from 'svelte';
	import Header from './Header.svelte';
	import './styles.css';

	onMount (() => {
		window.addEventListener('scroll', () => {
			const mainContainer = document.getElementById('main');
			mainContainer?.style.setProperty('background-position-y', `${window.scrollY}px`);
			console.log(window.scrollY);
		})
	});

</script>

<div class="app">
	<Header />

	<main>
		<div id='main' class='blur-bg'></div>
		<slot />
	</main>

	<footer>
		<p>Monitoring Dashboard by <a href="http://www.trisonworld.com"> TRISON </a>© 1998 - 2022 </p>
	</footer>
</div>

<style>
	/**center the main*/
	main {
		display: flex;
		justify-content: center;
		align-items: center;


	}
	.blur-bg {
		display: flex;
		justify-content: center;
		align-items: center;
		position: absolute;
		top: 100px;
		left: 0;
		width: 100%;
		height: 100%;
		background-image: url('/src/lib/images/porsche.png');
		background-repeat: no-repeat;
		background-position-x: center;
		background-size: 50%;
		filter: blur(5px);
		z-index: -1;
	}

</style>
