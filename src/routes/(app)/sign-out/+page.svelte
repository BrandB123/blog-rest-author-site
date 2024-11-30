<script>
    import { goto } from '$app/navigation';

    // testing async UI. Adding API call later
    const signedOut = new Promise((resolve, reject) => {
        setTimeout(() => {
            resolve('successfully');
            // return reject('failure');
            setTimeout(() => {goto('/log-in')}, 1000);
        }, 1500);
    });

</script>

{#await signedOut}
    <p>Wait a moment while you are signed out...</p>
{:then signedOut}
    <p>You have been signed out {signedOut}</p>
{:catch error}
    <p>Error signing out</p>
{/await}