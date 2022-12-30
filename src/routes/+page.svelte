<script lang="ts">
	import axios from 'axios';
	import Host from './Host.svelte';
	const zabbixApiUrl = 'http://20.229.182.95:9080//api_jsonrpc.php';

	let hosts: Array<any> = [];

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
						output: ['hostid', 'name', 'status'],
						selectInterfaces: ['interfaceid', 'ip'],
						selectTriggers: [
							'triggerid',
							'description',
							'expression',
							'priority',
							'status',
							'value'
						],
						selectItems: ['name', 'status', 'key_', 'value_type', 'valuemapid', 'units', 'lastvalue'],
						selectHttpTests: ['httptestid', 'name'],
						selectMacros: ['macro', 'value'],
						selectInventoryMode: ['inventory_mode'],
						selectDiscoveryRule: ['itemid', 'name'],
						selectItemDiscovery: ['itemid', 'name'],
						selectTriggersDiscovery: [
							'triggerid',
							'description',
							'expression',
							'priority',
							'status',
							'value'
						],
						selectGraphDiscovery: ['graphid', 'name'],
						selectHostDiscovery: ['hostid', 'name', 'status'],
						selectLatestItems: ['itemid', 'name', 'key_'],
						selectIcon: ['iconid', 'name'],
						selectProfile: ['name', 'idx', 'source', 'value']
					},
					auth: '712d00c487267e61984018e1528fa4b735819c9666a3d2cf3d628eee66a1185b',
					id: 1
				})
				.then((response) => {
					hosts = response.data.result;
					console.log('response:', response.data.result);
				})
				.catch((error) => {
					console.log('error:', error);
				});
		});
</script>

<svelte:head>
	<title>Home</title>
	<meta name="description" content="Trison Zabbix" />
</svelte:head>

<section>
	{#each hosts as host}
		<Host name={host.name} ip={host.interfaces[0].ip} status={host.items[1].lastvalue}/>
	{/each}
</section>

<style>
	section {
		display: flex;
		flex-wrap: wrap;
		justify-content: center;
		align-items: center;
	}

</style>
