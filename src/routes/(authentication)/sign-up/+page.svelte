<script lang="ts">
  import { goto } from '$app/navigation';

    let firstName = $state("");
    let lastName = $state("");
    let name = $derived(`${firstName} ${lastName}`);
    let email = $state("");
    let password = $state("");

    async function addUser(name: string, email: string, password: string){
        try {
            const response = await fetch("http://localhost:3000/api/signup", {
                method: "POST",
                body: JSON.stringify({
                    name,
                    email, 
                    password, 
                    authorStatus: true
                }),
                headers: {
                    "Content-Type": "application/json"
                }
            });
            
            if (response.status === 422){ 
                alert("Name, Email, and Password must be provided")
            }

            if (response.status === 409){
                alert("A user with this email already exists.")
            }
            
            if (!response.ok){
                throw new Error("Response: ", response.status);
            }

            goto('/log-in')

        } catch(err) {
            console.error(err);

            if (err.name === 'TypeError'){
                alert("Service currently unavailable. Sorry for the inconvenience. Please try again later.")
            }
        }
    }
</script>

<h1 class="text-3xl pl-4">Create an Account</h1>
<form action="/"
    class="bg-neutral-100 w-1/4 mx-auto p-5 rounded-lg border border-neutral-500">

    <label for="first-name">First Name:</label>
    <input 
        type="text"
        id = "first-name"
        required 
        bind:value={firstName} 
        class="block">

    <label for="last-name">Last Name:</label>
    <input 
        type="text"
        id = "last-name"
        required 
        bind:value={lastName} 
        class="block">

    <label for="email">Email:</label>
    <input 
        type="text"
        id = "email"
        required 
        bind:value={email} 
        class="block">

    <label for="password">Password:</label>
    <input 
        type="password"
        id = "password"
        required 
        minlength="8"
        bind:value={password} 
        class="block">

    <button type="submit"
        class="p-1 border rounded-lg border-blue-600 bg-blue-500 text-neutral-200"
        onclick={(event) => {
            event.preventDefault();
            if (!/^[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}$/.test(email)){
                alert("Invalid email entry.")
            } else if (!/^(\S+)(\s*)(\S+)$/.test(name)){
                alert("Invalid name entry. First and last name required")
            } else {
                addUser(name, email, password)
            }
        }}>
        Create Account
    </button>
</form>