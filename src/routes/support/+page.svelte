<script lang="ts">
	let supportMail = "support@trisonworld.com";
</script>

<svelte:head>
	<title>About</title>
	<meta name="description" content="Technical support" />
</svelte:head>

<div class="text-column">
	<h1>Technical support</h1>
	<p>
		For any technical support, please contact us at <a href="mailto:{supportMail}">{supportMail}</a>
	</p>

	<div class="space"></div>
</div>

<style>
	.text-column {
		display: flex;
		flex-direction: column;
		justify-content: center;
		align-items: center;
		padding: 2rem;
	}

	.text-column h1 {
		font-size: 2rem;
		font-weight: 600;
	}

	.text-column p {
		font-size: 1.5rem;
		font-weight: 400;
	}

	.text-column a {
		color: #000;
	}

	.space {
		height: 20rem;
	}
</style>