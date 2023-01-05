<script lang="ts">
	import axios from 'axios';
	import Host from './Host.svelte';
	import HostInfo from './HostInfo.svelte';
	const zabbixApiUrl = 'http://20.229.182.95:9080//api_jsonrpc.php';

	let hosts: Array<any> = [];

	let groups: Array<any> = [];

	axios
		.post(zabbixApiUrl, {
			jsonrpc: '2.0',
			method: 'user.login',
			params: {
				user: 'Admin',
				password: 'zabbix'
			},
			id: 1,
			auth: null
		})
		.then((response) => {
			let auth = response.data.result;
			axios
				.post(zabbixApiUrl, {
					jsonrpc: '2.0',
					method: 'host.get',
					params: {
						output: ['active_available', 'name'],
						selectInterfaces: ['ip'],
						selectItems: ['name', 'lastvalue'],
						selectGroups: ['name']
					},
					auth: '712d00c487267e61984018e1528fa4b735819c9666a3d2cf3d628eee66a1185b',
					id: 1
				})
				.then((response) => {
					hosts = response.data.result;
					console.log('hosts:', hosts);
				})
				.catch((error) => {
					console.log('error:', error);
				});
			axios
				.post(zabbixApiUrl, {
					jsonrpc: '2.0',
					method: 'hostgroup.get',
					params: [],
					id: 1,
					auth: '712d00c487267e61984018e1528fa4b735819c9666a3d2cf3d628eee66a1185b'
				})
				.then((response) => {
					groups = response.data.result;
					console.log('hostgroups:', response.data.result);
				})
				.catch((error) => {
					console.log('error:', error);
				});
		});
	let shallShowModal = false;
	let currentHost: any = null;
	function showModal(host: any) {
		shallShowModal = true;
		currentHost = host;
	}

	function filterHosts() {
		let filter = document.querySelector('#group-select') as HTMLSelectElement;
		let selectedGroup = filter.value;
		axios
			.post(zabbixApiUrl, {
				jsonrpc: '2.0',
				method: 'host.get',
				params: {
					output: ['active_available', 'name'],
					selectInterfaces: ['ip'],
					selectItems: ['name', 'lastvalue'],
					selectGroups: ['name']
				},
				auth: '712d00c487267e61984018e1528fa4b735819c9666a3d2cf3d628eee66a1185b',
				id: 1
			})
			.then((response) => {
				hosts = response.data.result.filter(
					(host) => host.groups.filter((group) => group.name === selectedGroup).length > 0
				);
			}
			);
	}
</script>

<svelte:head>
	<title>Home</title>
	<meta name="description" content="Trison Zabbix" />
</svelte:head>

<section>
	<div id="modal">
		{#if shallShowModal}
			<HostInfo host={currentHost} />
		{/if}
	</div>
	<div class="filter-combobox" />
	<div class="head-data">
		<div class="device-count">Devices: {hosts.length}</div>
		<div class="status-count">
			<span class="unavailable"
				>{hosts.filter(
					(host) =>
						host.items.filter((item) => item.name === 'Zabbix agent ping' && item.lastvalue === '0')
							.length > 0
				).length}</span
			>
			<span class="available"
				>{hosts.filter(
					(host) =>
						host.items.filter((item) => item.name === 'Zabbix agent ping' && item.lastvalue === '1')
							.length > 0
				).length}</span
			>
		</div>
	</div>
	<div class="select-filter">
		<select id="group-select" on:change={filterHosts}>
			<option value="">Select a group</option>
			{#each groups as group}
				<option value={group.name}>{group.name}</option>
			{/each}
		</select>
	</div>
	<div class="hosts">
		{#each hosts as host}
			<div class="host" on:click={() => showModal(host)} on:keydown={() => showModal(host)}>
				<Host {host} />
			</div>
		{/each}
	</div>
</section>

<style>
	.head-data {
		display: flex;
		justify-content: space-between;
		align-items: space-between;
		padding: 2rem;
		width: 100%;
	}

	.device-count {
		font-size: 1.5rem;
		font-weight: 600;
	}

	.status-count {
		display: flex;
	}

	.status-count span {
		margin-right: 1rem;
	}

	.status-count span::after {
		content: ' ';
		display: inline-block;
		width: 1rem;
		height: 1rem;
		border-radius: 30%;
		margin-left: 0.5rem;
	}

	.status-count .unavailable::after {
		background-color: var(--offline);
	}

	.status-count .available::after {
		background-color: var(--online);
	}

	/*
	* Each host, same size in columns and rows, show at least 2 columns
	*/

	.hosts {
		display: grid;
		grid-template-columns: repeat(auto-fill, minmax(300px, 1fr));
		grid-gap: 1rem;
		padding: 1rem;
	}

	#modal {
		position: sticky;
		top: 0;
		left: 0;
		width: 100%;
		height: 100%;
		z-index: 100;
		display: flex;
		justify-content: center;
		align-items: center;
	}
</style>
