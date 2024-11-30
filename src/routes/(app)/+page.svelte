<script>
    let posts = []
</script>

<h1 class="text-3xl pl-4">Posts</h1>
{#each posts as post}
    <div>
        <h4>{post.title}</h4>
        <p>{post.message}</p>
    </div>
{:else}
    <p>No blog posts currently. Add a post to view it here</p>
{/each}
