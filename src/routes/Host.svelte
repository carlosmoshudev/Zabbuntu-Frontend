<script lang="ts">
	export let name: string;
	export let ip: string;
	export let status: string;
</script>

<div class="host">
	<h2 class="host-name">{name}</h2>
	<!--If status is 1 then it's online-->
	{#if status == '1'}
		<span>Status:</span> <strong class="online">ONLINE</strong>
	{:else}
		<span>Status:</span> <strong class="offline">OFFLINE</strong>
	{/if}
</div>

<style>
    @import url('https://fonts.googleapis.com/css2?family=Montserrat:wght@400;700&display=swap');
	.host {
        font-family: 'Montserrat';
        font-weight: extralight;
        width: 350px;
		height: 100px;
		margin: 10px;
		padding: 10px;
        border-radius: 35px;
        background: #dadada;
        box-shadow:  13px -13px 20px #c2c2c2;
		/**box-shadow: 0 1px 2px 0 rgba(0, 0, 0, 0.2), 0 2px 10px 0 rgba(0, 0, 0, 0.19);**/
	}

	/*
    *   Host name is bold and colored, centered and top aligned
    */
	.host-name {
		color: #ffffff;
		text-align: center;
		vertical-align: top;
		border-bottom: 1px solid #ffffff;	}
	span {
		font-weight: bold;
		color: var(--color-theme-2);
	}
	strong {
		color: var(--color-theme-1);
	}
	.online {
		color: var(--online);
        text-shadow: #5e5e5e 0px 0px 5px;
	}
	.offline {
		color: var(--offline);
	}
	.unknown {
		color: var(--unknown);
	}
</style>
