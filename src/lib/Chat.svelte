<script>

    import {Collection} from "sveltefire"
    import {collection, addDoc, onSnapshot} from "firebase/firestore"

    export let auth
    export let db
    export let userInfo

    let message = ""

    let sortingFunction = (x, y) => {
        if (x.date.seconds > y.date.seconds) {
            return 1
        } else if (x.date.seconds < y.date.seconds) {
            return -1
        } else {
            return 0
        }
    }

    let sendMessage = () => {

        let messageCollection = collection(db, "messages")
        addDoc(messageCollection, {
            text: message,
            firstName: userInfo.firstName,
            lastName: userInfo.lastName,
            date: new Date(),
        })

        message = ""

    }

    let messageDiv

    let messageCollection = collection(db, "messages")
    onSnapshot(messageCollection, async (snapshot) => {
        await new Promise(resolve => setTimeout(resolve, 10));
        messageDiv.scrollTop = messageDiv.scrollHeight
    });

</script>

<style>

    #chatDiv {

        padding: 0.5rem;

        box-shadow: 2px 2px 10px grey;
        background-color: white;

        width: 38%;
        height: 100%;

        display: flex;
        flex-flow: column;
        align-items: center;

       

    }

    #messageDiv {
        height: 80%;
        overflow-y: scroll;
        flex-grow: 4;

        display: flex;
        flex-direction: column;
        align-items: left;

        width: 90%;

    }
   

    h1 {
        margin: 0;
        font-weight: 200;
        font-style: italic;

    }

    input {
        font-size: 1rem;
        border-radius: 5px;
        border: solid grey 2px;
        
    }

    button {
        border-radius: 5px;
        font-size: 1rem;
        background-color: whitesmoke;
       
        /* border: grey solid 2px; */
        
        transition: background-color 0.4s;
    }

    button:hover {
        background-color: lightgreen;
    }
    

    .message {
        margin: 0.25rem;
        padding: 0.25rem;
        
        
    }

    

    p {
        padding: 0;
        margin: 0;
    }

</style>


<div id="chatDiv">
    
    <h1>Chat</h1>
    
    <div bind:this={messageDiv} id="messageDiv">

        <Collection ref={'messages'} let:data let:count>

            
            {#each data.sort(sortingFunction) as message}
                <div class="message">
                    <p><b>{message.firstName + " " + message.lastName}:</b></p>
                    <p>{message.text}</p>
                </div>
                
            {/each}
        
            
        </Collection>     
    
    </div>
    
    <div>
        
        <input bind:value={message} type="text">
        <button on:click={sendMessage}>Send</button>

    </div>
        

        
</div>