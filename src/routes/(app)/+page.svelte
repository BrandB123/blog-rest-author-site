<script lang="ts">
    import { onMount } from "svelte";
    import { goto } from "$app/navigation";

    let posts;

    async function getPosts(){
        try {
            const response = await fetch(`${import.meta.env.VITE_API_URL}api/posts/author`, {
                headers: {
                    "Content-Type" : "application/json",
                    "Authorization" : `Bearer ${localStorage.getItem("authorjwt")}`, 
                }
            })

            if (response.status === 401){
                goto('/log-in');
            }

            if (response.status === 403){
                goto('/sign-up');
            }

            if (!response.ok){
                throw new Error("Response Status: ", response.status)
            }

            const json = await response.json();

            console.log("RESPONSE: ", response);
            console.log("JSON: ", json);

            return json.posts

        } catch(err){
            console.error(err);
        }
    }


    onMount(async () => {
        posts = getPosts();
        console.log(posts);
    });
        
</script>

<h1 class="text-4xl pl-4 mt-4">Posts</h1>

{#await posts}
<p>Loading posts...</p>

{:then posts}
    {#each posts as post}
        <div
        class="border border-black rounded-xl my-4 mx-8 p-4 bg-neutral-200 relative">
            <h4 class="font-bold text-xl">
                {post.title}
            </h4>
            <p class="text-sm">
                {post.message}
            </p>
            <a
            href="/{post.title.split(" ").join("-").toLowerCase()}/"
            class="m-2 p-1 text-xs absolute right-0 top-0 hover:font-bold">
                Edit
            </a>
        </div>
    {:else}
        <p>No blog posts currently. Add a post to view it here</p>
    {/each}
{:catch error}
    <p>Error loading posts. Please try again later</p>
{/await}
