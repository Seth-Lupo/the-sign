<script>

    import Chat from "./Chat.svelte";
    import { doc, setDoc, updateDoc } from "firebase/firestore";

    import { Doc, userStore } from "sveltefire"

    export let db
    export let auth
    export let userInfo

    let newSignText = ""

    let changeSign = async () => {

        let signDoc = await doc(db, `sign/signInfo`)

        setDoc(signDoc, {
            text: newSignText,
            author: userInfo.firstName,
            userID: auth.currentUser.uid,
            date: new Date(),
        })

        let userDoc = await doc(db, `userInfo/${auth.currentUser.uid}`)

        updateDoc(userDoc, {
            signLastEditDate: new Date(),
        })

        newSignText = ""

    }

    

    var remainingCooldown

    setInterval(() => {

        let nextPostSeconds = userInfo.signLastEditDate.seconds + 3600
        let currentTimeSeconds = Math.floor((new Date().getTime())/1000)

        remainingCooldown = Math.max(Math.floor((nextPostSeconds - currentTimeSeconds)/60), 0)
    
    }, 1000);
    
    

</script>


<style>

    #outerDiv {

        height: 60vh;
        aspect-ratio: 16 / 9;

        position: absolute;
        top: 50vh;
        left: 50vw;
        transform: translate(-50%, -50%);

        display: flex;

        align-items: center;
        justify-content: space-around;

    }

    #signDiv {

        display: relative;

        display: flex;
        flex-direction: column;
        align-items: center;
        justify-content: center;
       

        box-shadow: 2px 2px 10px grey;
        background-color: white;


        width: 60%;
        height: 100%;
        
        
    }

    h1 {
        font-size: 4rem;
        text-align: center;
    }


    #signEditDiv {
        position: absolute;
        bottom: 1rem;
    }

    input {
        font-size: 1.5rem;
        border-radius: 5px;
        border: solid grey 2px;
        
    }

    button {
        border-radius: 5px;
        font-size: 1.5rem;
        background-color: whitesmoke;
       
        /* border: grey solid 2px; */
        color: green;
        transition: background-color 0.4s;
    }

    .active {
        color: green;
    }

    button.active:hover {
        background-color: lightgreen;
    }

    .inactive {
        color: red;
    }

    button.inactive:hover {
        
    }



    

</style>

<main>

    <div id="outerDiv">
        <div id="signDiv">

            <Doc ref={`sign/signInfo`} let:data={signInfo}>

                <h1>{signInfo.text}</h1>
                <h2>— {signInfo.author}</h2>

                <div  id="signEditDiv">
                    
                    <input bind:value={newSignText} type="text">
                    
                   
                        {#if remainingCooldown == 0}
                            <button on:click={changeSign} class="active">Change Now</button>
                        {:else} 
                            <button on:click={changeSign} class="inactive" disabled>Wait {remainingCooldown} min</button>
                        {/if}

                       
                   

                

                    
                </div>
                
            </Doc>
            



        </div>

        
        <Chat {db} {auth} {userInfo}/>
        
        
    </div>

</main>