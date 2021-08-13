<script>
    import { API_URL } from "$lib/env";
    import { onMount } from "svelte";

    let albums = [];
    const getAlbums = async () => {
        const res = await fetch(`${API_URL}/albums`);
        const json = await res.json();
        albums = json.data;
    };
    onMount(getAlbums);
</script>

<h1>List of Albums</h1>
<table class="table table-striped">
    <thead>
        <tr>
            <td>AlbumId</td>
            <td>Title</td>
            <td>ArtistId</td>
        </tr>
    </thead>
    <tbody>
        {#each albums as album (album.AlbumId)}
            <tr>
                <td>{album.AlbumId}</td>
                <td>{album.Title}</td>
                <td>{album.ArtistId}</td>
                <td
                    ><a
                        href={`/admin/albums/${album.AlbumId}`}
                        class="btn btn-primary">Details</a
                    ></td
                >
            </tr>
        {/each}
    </tbody>
</table>
