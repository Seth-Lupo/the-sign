<script>

    import { signOut } from "firebase/auth";

    export let auth
    export let db

    let logOut = () => {
        signOut(auth)
    }


</script>

<style>
    button {
        font-size: medium;
    }
</style>

<button on:click={logOut}>Log Out</button>