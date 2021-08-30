<script>
    import { API_URL } from "$lib/env";
    import { onMount } from "svelte";

    let tracks = [];
    const getTracks = async () => {
        const res = await fetch(`${API_URL}/tracks`);
        const json = await res.json();
        tracks = json.data;
    };
    const handleDelete = async (id) => {
        try {
            const res = await fetch(`${API_URL}/tracks/${id}`, {
                method: "DELETE",
            });
            if (res.status == 200) {
                await getTracks();
            }
            if (res.status == 419) {
                alert("Record Can't be deleted");
            }
        } catch (error) {
            console.log(
                "🚀 ~ file: index.svelte ~ line 23 ~ handleDelete ~ error",
                error
            );
        }
    };
    onMount(getTracks);
</script>

<h1>List of Tracks</h1>
<table class="table table-striped">
    <thead>
        <tr>
            <td>Track</td>
            <td>Album</td>
            <td>Artist</td>
            <td>Composer</td>
            <td>UnitPrice</td>
            <td>Actions</td>
        </tr>
    </thead>
    <tbody>
        {#each tracks as track (track.TrackId)}
            <tr>
                <td>{track.Name}</td>
                <td>{track.Album.Title}</td>
                <td>{track.Artist.Name}</td>
                <td>{track.Composer}</td>
                <td>{track.UnitPrice}</td>
                <td
                    ><a
                        href={`/admin/tracks/${track.TrackId}`}
                        class="btn btn-primary">Details</a
                    >
                    <button
                        class="btn btn-danger"
                        on:click={() => handleDelete(track.TrackId)}
                        >Delete</button
                    >
                </td>
            </tr>
        {/each}
    </tbody>
</table>
