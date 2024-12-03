<script lang="ts">
    let title = $state("");
    let message = $state("");

    /*
        fetch post to /author/
        headers: json, authentication
        body: postTitle, postMessage, publishedStatus


        verify user

        add post for author
        async function addPostForAuthor(req, res, next){
    if (!req.user.author_role){
        return res.status(403).json({ message: "Access Denied - User Role Invalid"})
    }
    if (!req.body.postTitle || !req.body.postMessage || !req.body.publishedStatus){
        return res.status(422).json({ message: "Missing required fields"})
    }
    const posts = await db.getPostsForAuthor(req.user.id)

    const postTitles = posts
        .filter(post => post.id !== Number(req.user.id))
        .map(post => post.title)
    
    if (postTitles.includes(req.body.postTitle)){
        return res.status(409).json({ message : "Post title must be unique"})
    }

    await db.addPostForAuthor(req.user.id, req.body.postTitle, req.body.postMessage, req.body.publishedStatus)
    res.json({ message : "Post added successfully"});
}
    
    */



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

    <button
        type="submit"
        class="border border-black rounded-xl p-1 w-1/6 mx-auto bg-blue-500 text-neutral-100 text-xl"
        onclick={(event) => {
            event.preventDefault();
            console.log(`TITLE: ${title}, MESSAGE: ${message}`)
        }}>
        Submit
    </button>
</form>