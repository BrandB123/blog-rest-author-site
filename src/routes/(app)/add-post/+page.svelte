<script lang="ts">
    import { goto } from "$app/navigation";
  
    let title = $state("");
    let message = $state("");
    let published = $state(false);
  
    async function addPost(title: string, message: string, published: boolean){
        try {
            const response = await fetch(`${import.meta.env.VITE_API_URL}api/posts/author`, {
                method: "POST",
                headers: {
                    "Content-Type": "application/json",
                    "Authorization": `Bearer ${localStorage.getItem("jwtAuth")}`
                },
                body: JSON.stringify({
                    postTitle: title,
                    postMessage: message,
                    publishedStatus: published
                })
            });
        
            if (response.status === 401){
                goto('/log-in');
            }

            if (response.status === 403){
                goto('/sign-up');
            }

            if (response.status === 422){ 
                alert("Please complete all fields.")
            }

            if (response.status === 409){
                alert("You have already created a post with this title. Post titles must be unique.")
            }
        
            if (!response.ok){
                throw new Error("Response Status: ", response.status);
            }
        
            goto("/");
    
        } catch(err){
            console.error(err);
        }
    } 
</script>

<h1 class="text-4xl pl-4 mt-4">Add Post</h1>
<form 
    action="/"
    class="bg-neutral-200 m-8 p-5 rounded-xl flex flex-col border border-neutral-500">

    <label 
        for="title"
        class="text-xl mb-2">
        Post Title:
    </label>
    <input 
        type="text" 
        id="title"
        bind:value={title}
        class="rounded-lg border border-neutral-400 mb-4 px-4 py-2">

    <label 
        for="message"
        class="text-xl mb-2">
        Post Message:
    </label>
    <textarea 
        id="message" 
        rows="10" 
        cols="80"
        bind:value={message}
        class="rounded-lg border border-neutral-400 mb-4 px-3 py-2">
    </textarea>

    <div class="ml-4">
        <input
            type="radio"
            id="publish"
            name="publish-status"
            bind:group={published}
            value="true"
            class="inline">
        <label
            for="publish"
            class="inline">
            Publish
        </label>
    </div>

    <div class="ml-4">
        <input
            type="radio"
            id="archive"
            name="publish-status"
            checked
            bind:group={published}
            value="false"
            class="inline">
        <label
            for="archive"
            class="inline">
            Archive
        </label>
    </div>

    <button
        type="submit"
        class="border border-black rounded-xl p-1 w-1/2 md:w-1/4 lg:w-1/6 mt-2 md:mt-0 mx-auto bg-blue-500 text-neutral-100 text-xl"
        onclick={(event) => {
            event.preventDefault();

            if (title.length > 255){
                alert("Title can only be 255 characters.")
            } else if (message.length > 3000){
                alert("Message can only be 3000 characters.")
            } else if (!/^(.+)(\S+)(.+)$/.test(title) || !/^(.+)(\S+)(.+)$/.test(message)){
                alert("Invalid title or message entry. Please try again")
            } else {
                addPost(title, message, published);
            }
        }}>
        Submit
    </button>
</form>