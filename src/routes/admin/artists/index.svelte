<script>
    import { API_URL } from "$lib/env";
    import { onMount } from "svelte";

    let artists = [];
    const getArtists = async () => {
        const res = await fetch(`${API_URL}/artists`);
        const json = await res.json();
        artists = json.data;
    };
    const handleDelete = async (id) => {
        try {
            const res = await fetch(`${API_URL}/artists/${id}`, {
                method: "DELETE",
            });
            if (res.status == 200) {
                await getArtists();
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
    onMount(getArtists);
</script>

<h1>List of Artists</h1>

<table class="table table-striped">
    <thead>
        <tr>
            <th>ArtistId</th>
            <th>Name</th>
            <th>Actions</th>
        </tr>
    </thead>
    <tbody>
        {#each artists as artist (artist.ArtistId)}
            <tr>
                <td>{artist.ArtistId}</td>
                <td>{artist.Name}</td>
                <td
                    ><a
                        href={`/admin/artists/${artist.ArtistId}`}
                        class="btn btn-primary">Details</a
                    >
                    <button
                        class="btn btn-danger"
                        on:click={() => handleDelete(artist.ArtistId)}
                        >Delete</button
                    >
                </td>
            </tr>
        {/each}
    </tbody>
</table>
