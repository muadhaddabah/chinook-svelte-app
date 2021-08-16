<script>
    import { API_URL } from "$lib/env";
    import { onMount } from "svelte";

    let playlists = [];
    const getPlaylists = async () => {
        const res = await fetch(`${API_URL}/playlists`);
        const json = await res.json();
        playlists = json.data;
    };
    const handleDelete = async (id) => {
        try {
            const res = await fetch(`${API_URL}/playlists/${id}`, {
                method: "DELETE",
            });
            if (res.status == 200) {
                await getPlaylists();
            }
            if (res.status == 419) {
                alert("Record Can't be deleted");
            }
        } catch (error) {
            console.log(
                "🚀 ~ file: index.svelte ~ line 17 ~ handleDelete ~ error",
                error
            );
        }
    };
    onMount(getPlaylists);
</script>

<h1>List of Playlists</h1>

<table class="table table-striped">
    <thead>
        <tr>
            <td>PlaylistId</td>
            <td>Name</td>
        </tr>
    </thead>
    <tbody>
        {#each playlists as playlist (playlist.PlaylistId)}
            <tr>
                <td>{playlist.PlaylistId}</td>
                <td>{playlist.Name}</td>
                <td
                    ><a
                        href={`/admin/playlists/${playlist.PlaylistId}`}
                        class="btn btn-primary">Details</a
                    >
                    <button
                        class="btn btn-danger"
                        on:click={() => handleDelete(playlist.PlaylistId)}
                        >Delete</button
                    >
                </td>
            </tr>
        {/each}
    </tbody>
</table>
