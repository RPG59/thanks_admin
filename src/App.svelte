<script>
	import { Router, Route, Link } from "svelte-routing";
	import SignInSignUp from "./pages/SignInSignUp.svelte";
	import CreatePartner from "./pages/CreatePartner.svelte"
	import PartnersList from "./pages/PartnersList.svelte"
	export let url = "";
</script>

<Router url={url}>
	<nav>
		<Link to="/">Home</Link>
		<Link to="sign?state=in">Sign in</Link>
		<Link to="sign?state=up">Sign up</Link>
		<Link to="/create-partner">Create Partner</Link>
		<Link to="/partners-list">Partners List</Link>
	</nav>
	<div>
		<Route path="sign" component={SignInSignUp}/>
		<Route path="create-partner" component={CreatePartner}/>
		<Route path="partners-list" component={PartnersList}/>
		<Route path="/">
			<h1>test</h1>
		</Route>
	</div>
</Router>