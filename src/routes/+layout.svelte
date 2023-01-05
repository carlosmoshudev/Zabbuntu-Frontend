<script>
	import { onMount } from 'svelte';
	import Header from './Header.svelte';
	import './styles.css';
	/*
	*	Using bootstrap 5.0.2
	*/

	import 'bootstrap/dist/css/bootstrap.min.css';

	onMount (() => {
		window.addEventListener('scroll', () => {
			const mainContainer = document.getElementById('main');
			mainContainer?.style.setProperty('background-position-y', `${window.scrollY}px`);
		})
	});

</script>

<div class="app">
	<div class="header"><Header /></div>

	<main>
		<div id='main' class='blur-bg'></div>
		<slot />
	</main>

	<footer>
		<p>Monitoring Dashboard by <a href="/about"> TRISON </a>© 1998 - 2022 </p>
	</footer>
</div>

<style>
	.header {
		top: 0px;
		width: 100%;
		height: 100px;
		background-color: var(--color-theme-3);
	}
	.app {
		display: flex;
		flex-direction: column;
		height: 100vh;
		background-color: #ffffff00;
	}
	/**center the main*/
	main {
		display: flex;
		justify-content: center;
		align-items: center;
		width: 100%;
		background-color: var(--color-theme-3);
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
		filter: blur(4px);
		z-index: -1;
		background-color: #ffffff00;

	}

	footer {
		position: sticky;
		bottom: 0px;
		width: 100%;
		height: 20px;
		background-color: var(--color-bg-1);
		display: flex;
		justify-content: center;
		align-items: center;
		font-family: 'Montserrat', sans-serif;
	}

	footer p {
		font-size: 0.7rem;
		font-weight: 400;
		letter-spacing: 4px;
		padding-top: 2px;
		background-color: #ffffff00;

	}

	footer a {
		color: var(--color-theme-1);
		font-weight: 600;
		background-color: #ffffff00;

	}
</style>
