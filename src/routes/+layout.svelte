<script>
    
    import { FirebaseApp, SignedIn, User, Doc } from 'sveltefire'
    import { initializeApp} from 'firebase/app'
    import { getFirestore } from 'firebase/firestore'
    import { getAuth} from 'firebase/auth'
    import { SignedOut } from 'sveltefire';

    import Authenticator from "$lib/Authenticator.svelte"
    import LogOut from '$lib/LogOut.svelte';
  import Sign from '$lib/Sign.svelte';


    const firebaseConfig = {
        apiKey: "AIzaSyBeOD4ZiRp4YvpqBXuL2jmfqNmulPAPfQY",
        authDomain: "the-sign-13549.firebaseapp.com",
        projectId: "the-sign-13549",
        storageBucket: "the-sign-13549.appspot.com",
        messagingSenderId: "397074912216",
        appId: "1:397074912216:web:a953ba8e5be031b73418ad",
        measurementId: "G-D9DR93TFCS"
    };
    
    const app = initializeApp(firebaseConfig);
    const db = getFirestore(app);
    const auth = getAuth(app);



</script>

<style>
    :root {
        font-family:'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
        background-color: rgb(245 245 245);
    }

    :global(*) {
        box-sizing: border-box;
    }


    

</style>



<FirebaseApp firestore={db} auth={auth}>
   
        <SignedOut>
            <Authenticator db={db} auth={auth} existingAccount={true}/>
        </SignedOut>

        <SignedIn let:user>

            <Doc ref={`userInfo/${user.uid}`} let:data={userInfo}>
                <h1>Hello {userInfo.firstName} 👋</h1>
                <LogOut {db} {auth} />
                <Sign {db} {auth} {userInfo}></Sign>
            </Doc>

                  

        </SignedIn>

        <slot/>
    
</FirebaseApp>
