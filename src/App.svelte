<script>
    import Router from 'svelte-spa-router';

	import Home from './pages/Home.svelte';
	import Login from './pages/Login.svelte';
	import Admins from './pages/Admins.svelte';
	import AdminsAdd from './pages/AdminsAdd.svelte';
	import AdminsEdit from './pages/AdminsEdit.svelte';
	import Customers from './pages/Customers.svelte';
	import CustomersAdd from './pages/CustomersAdd.svelte';
	import CustomersEdit from './pages/CustomersEdit.svelte';
	import PageNotFound from './pages/PageNotFound.svelte';

	import Topbar from './pages/components/Topbar.svelte';
	import Footer from './pages/components/Footer.svelte';

	let routes = {
		'/' : Home,
		'/home': Home,
		'/login': Login,
		'/admins': Admins,
		'/admins/add': AdminsAdd,
		'/admins/edit/:id': AdminsEdit,
		'/customers': Customers,
		'/customers/add': CustomersAdd,
		'/customers/edit/:id': CustomersEdit,


		'*': PageNotFound
	}

</script>

<main>
	<Topbar {Topbar}></Topbar>
	<Router {routes} />
	<Footer {Footer}></Footer>
</main>

<style>
	main {
		text-align: center;
		max-width: 240px;
		margin: 0 auto;
	}

	@media (min-width: 640px) {
		main {
			max-width: none;
			width: 100%;
		}
	}
</style>