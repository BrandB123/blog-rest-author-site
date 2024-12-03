<script lang="ts">
    import { goto } from "$app/navigation";

    let username = $state("");
    let password = $state("");

    async function login(username: string, password: string){
        try {
            const response = await fetch(`${import.meta.env.VITE_API_URL}api/login`, {
                method: "POST",
                body: JSON.stringify({
                    username,
                    password
                }),
                headers: {
                    "Content-Type": "application/json"
                }
            });

            if (response.status === 401){
                alert("Invalid login credentials.")
            }

            if (!response.ok){
                throw new Error("Response Status: ", response.status)
            }

            const json = await response.json();
            console.log("RESPONSE: ", response );
            console.log("JSON: ", json );
            console.log("TOKNE: ", json.token)

            localStorage.setItem("authorjwt", json.token)
            goto('/')
            
        } catch(err){
            console.error(err);

            if (err.name === 'TypeError'){
                alert("Service currently unavailable. Sorry for the inconvenience. Please try again later.")
            }
        }
    }

</script>

<h1 class="text-3xl pl-4">Log In</h1>
<form action="/"
    class="bg-neutral-100 w-1/4 mx-auto p-5 rounded-lg border border-neutral-500">

    <label for="email">Email:</label>
    <input 
        type="text" 
        id="email" 
        bind:value={username}
        class="block">

    <label for="password">Password:</label>
    <input 
        type="password" 
        id="password" 
        bind:value={password}
        class="block">

    <button 
        type="submit"
        class="p-1 border rounded-lg border-blue-600 bg-blue-500 text-neutral-200"
        onclick={(event) => {
            event.preventDefault();
            console.log(`USERNAME: ${username}, PASSWORD: ${password}`)
            login(username, password);
        }}>
        Log in
    </button>
</form>