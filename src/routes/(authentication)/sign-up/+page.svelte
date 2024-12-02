<script>

    // const { name, email, password, authorStatus } = req.body
    let firstName = $state("testing");
    let lastName = $state("testing");

    let name = $derived(`${firstName} ${lastName}`);
    let email = $state("testing");
    let password = $state("testing");

    // on submit,
        // check inputs 
        // post request to http://localhost:3000/api/signup
        // if response 200, forward to sign in page
        // else display error page and direct back to the sign up page again after 10 seconds
    async function addUser(){
        try {
            const response = await fetch("http://localhost:3000/api/signup", {
                method: "POST",
                body: JSON.stringify({
                    name: {name},
                    email: {email}, 
                    password: {password}, 
                    authorStatus: true
                })
            })
        } catch(err) {

        }
    }
    
</script>

<h1 class="text-3xl pl-4">Create an Account</h1>
<form action="/"
    class="bg-neutral-100 w-1/4 mx-auto p-5 rounded-lg border border-neutral-500">
    <label for="">First Name:</label>
    <input type="text" name="" id="" bind:value={firstName}
    class="block">
    <label for="">Last Name:</label>
    <input type="text" name="" id="" value={lastName}
    class="block">
    <label for="">Email:</label>
    <input type="text" name="" id="" value={email}
    class="block">
    <label for="">Password:</label>
    <input type="password" name="" id="" value={password}
    class="block">
    <button type="submit"
    class="p-1 border rounded-lg border-blue-600 bg-blue-500 text-neutral-200"
    onclick={(event) => {
        console.log(`NAME: ${name}, EMAIL: ${email}, PASSWORD: ${password}`);
        event.preventDefault();
        }}>
        Create Account
    </button>
</form>