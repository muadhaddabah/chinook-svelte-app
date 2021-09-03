<script>
    import { API_URL } from "$lib/env";
    import { onMount } from "svelte";

    let albums = [];
    const getAlbums = async () => {
        const res = await fetch(`${API_URL}/albums/sz`);
        const json = await res.json();
        albums = json.data;
    };

    const handleDelete = async (id) => {
        try {
            const res = await fetch(`${API_URL}/albums/${id}`, {
                method: "DELETE",
            });

            if (res.status == 200) {
                await getAlbums();
            }
            if (res.status == 419) {
                alert("Record can't be deleted");
            }
        } catch (error) {
            console.log(
                "🚀 ~ file: index.svelte ~ line 26 ~ cosnthandleDelete ~ error",
                error
            );
        }
    };
    onMount(getAlbums);
</script>

<h1>List of Albums</h1>
<table class="table table-striped">
    <thead>
        <tr>
            <td>Title</td>
            <td>ArtistId</td>
            <td>Total Sales</td>
            <td>Actions</td>
        </tr>
    </thead>
    <tbody>
        {#each albums as album (`${album.AlbumId}_${album.Track.AlbumId}`)}
            <tr>
                <td>{album.Title}</td>
                <td>{album.ArtistId}</td>
                <td>{Math.round(album.totalSales * 100) / 100}</td>
                <td
                    ><a
                        href={`/admin/albums/${album.AlbumId}`}
                        class="btn btn-primary">Details</a
                    >
                    <button
                        class="btn btn-danger"
                        on:click={() => handleDelete(album.AlbumId)}
                        >Delete</button
                    >
                </td>
            </tr>
        {/each}
    </tbody>
</table>
