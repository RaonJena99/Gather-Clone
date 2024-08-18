<script>
  import Main from "./pages/Main.svelte";
  import Router from "svelte-spa-router";
  import "./css/reset.css";

  const routes = {
    "/": Main,
  };
</script>

<Router {routes}></Router>
