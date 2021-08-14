<script>
    import { API_URL } from "$lib/env";
    import { onMount } from "svelte";

    let artists = [];
    const getArtists = async () => {
        const res = await fetch(`${API_URL}/artists`);
        const json = await res.json();
        artists = json.data;
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
                <td>{artist.Name}</td>
                <td>{artist.ArtistId}</td>
                <td
                    ><a
                        href={`/admin/artists/${artist.ArtistId}`}
                        class="btn btn-primary">Details</a
                    ></td
                >
            </tr>
        {/each}
    </tbody>
</table>
