<script>

    import { createUserWithEmailAndPassword, signInWithEmailAndPassword } from "firebase/auth";
    import { doc, setDoc } from "firebase/firestore";

    export let auth
    export let db

    export let existingAccount

    let firstName = ''
    let lastName = ''
    let year = ''
    let school = ''

    let email = ''
    let password = ''

    

    let handleSubmit = async () => {
        if (existingAccount) {
            await signInWithEmailAndPassword(auth, email, password)
        } else {

            let userCredential = await createUserWithEmailAndPassword(auth, email, password)
            
            let userID = await userCredential.user.uid
            let userDoc = doc(db, `userInfo/${userID}`)
            setDoc(userDoc, {
                firstName: firstName,
                lastName: lastName,
                year: year,
                signLastEditDate: new Date(null),
            })

            await signInWithEmailAndPassword(auth, email, password)
        
        }
    }

    let toggleExistingAccount = () => {
        existingAccount = !existingAccount;
    }


</script>

<main>

    <h1>{existingAccount ? "Log In" : "Sign Up" }</h1>

    <div id="fields">
        
        
        
        <label>Email</label>
        <input type="text" bind:value={email}/>
        <label>Password</label>
        <input type="text" bind:value={password}/>

        {#if !existingAccount}
            <label>First Name</label>
            <input type="text" bind:value={firstName}/>
            <label>Last Name</label>
            <input type="text" bind:value={lastName}/>
            <label>Year</label>
            <input type="text" bind:value={year}/>
        {/if}


    </div>

    <button on:click={handleSubmit}>{existingAccount ? "Log In" : "Sign Up" }</button>

    <a on:click={toggleExistingAccount}><i>{existingAccount ? "Create an account." : "Log into an existing account." }</i></a>
    

</main>


<style>

    
    h1, button, a, label, input {
        font-family:'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
        font-size: 1.5rem;
    }

    #fields {
        display: flex;
        flex-direction: column;
        align-items: left;
        width: 30vw;
    }

    main {
        display: flex;
        flex-direction: column;
        align-items: center;
        justify-content: center;

        width: 100vw;
        height: 100vh;

    }

    a {
        
    }

    main > * {
        margin: 1rem;
    }


</style>