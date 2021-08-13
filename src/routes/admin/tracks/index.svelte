<script>
    import { API_URL } from "$lib/env";
    import { onMount } from "svelte";

    let tracks = [];
    const getTracks = async () => {
        const res = await fetch(`${API_URL}/tracks`);
        const json = await res.json();
        tracks = json.data;
    };
    onMount(getTracks);
</script>

<h1>List of Tracks</h1>
<table class="table table-striped">
    <thead>
        <tr>
            <td>TrackId</td>
            <td>Name</td>
            <td>AlbumId</td>
            <td>MediaTypeId</td>
            <td>GenreId</td>
            <td>Composer</td>
            <td>Milliseconds</td>
            <td>Bytes</td>
            <td>UnitPrice</td>
        </tr>
    </thead>
    <tbody>
        {#each tracks as track (track.TrackId)}
            <tr>
                <td>{track.TrackId}</td>
                <td>{track.Name}</td>
                <td>{track.AlbumId}</td>
                <td>{track.MediaTypeId}</td>
                <td>{track.GenreId}</td>
                <td>{track.Composer}</td>
                <td>{track.Milliseconds}</td>
                <td>{track.Bytes}</td>
                <td>{track.UnitPrice}</td>
                <td
                    ><a
                        href={`/admin/tracks/${track.TrackId}`}
                        class="btn btn-primary">Details</a
                    ></td
                >
            </tr>
        {/each}
    </tbody>
</table>
