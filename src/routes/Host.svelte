<script lang="ts">
	export let host: any;
</script>

<section>
	<div class="host">
		<div class="host-name">
			{host.name}
		</div>
		<div class="status">
			{#each host.items as item}
				{#if item.name === 'Zabbix agent ping'}
					{#if item.lastvalue === '0'}
						Status: <span class="offline">Offline</span>
					{:else if item.lastvalue === '1'}
						Status: <span class="online">Online</span>
					{:else}
						Status: <span class="unknown">{item.lastvalue}</span>
					{/if}
				{/if}
			{/each}
		</div>
		<div class="host-groups">
			{#each host.groups as group}
				{#if group.name !== 'Discovered hosts'}
					<div class="group-tag">
						{group.name}
					</div>
				{/if}
			{/each}
		</div>
	</div>
</section>

<style>
	@import url('https://fonts.googleapis.com/css2?family=Montserrat:wght@400;700&display=swap');

	section {
		display: flex;
		justify-content: center;
		align-items: center;
		height: 100%;
	}

	.host {
		display: flex;
		flex-direction: column;
		justify-content: space-between;
		align-items: space-between;
		width: 100%;
		max-width: 500px;
		padding: 0.8rem;
		border-radius: 0.8rem;
		background-color: var(--color-bg-2);
		box-shadow: 0 0 1rem rgba(0, 0, 0, 0.2);
	}

	.host-name {
		font-size: 1.5rem;
		font-weight: 600;
	}

	.status {
		font-size: 1.2rem;
		font-weight: 400;
	}

	.status span {
		font-weight: 600;
	}

	.status .offline {
		color: var(--offline);
	}

	.status .online {
		color: var(--online);
	}

	.status .unknown {
		color: var(--unknown);
	}
		
	.host-groups {
		font-size: 0.7rem;
		font-weight: 400;
		display: flex;
		flex-wrap: wrap;
		padding: 0rem 2rem;
		padding-top: 1rem;
		justify-content: space-between;
	}

	.group-tag {
		display: inline-block;
		padding: 0.2rem 0.4rem;
		margin-right: 0.4rem;
		border-radius: 0.4rem;
		background-color: var(--color-bg-1);
	}
</style>