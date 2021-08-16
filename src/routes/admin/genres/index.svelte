<script>
    import { API_URL } from "$lib/env";
    import { onMount } from "svelte";

    let genres = [];
    const getGenres = async () => {
        const res = await fetch(`${API_URL}/genres`);
        const json = await res.json();
        genres = json.data;
    };

    const handleDelete = async (id) => {
        try {
            const res = await fetch(`${API_URL}/genres/${id}`, {
                method: "DELETE",
            });
            if (res.status == 200) {
                await getGenres();
            }
            if (res.status == 419) {
                alert("Record Can't be deleted");
            }
        } catch (error) {
            console.log(
                "🚀 ~ file: index.svelte ~ line 24 ~ handleDelete ~ error",
                error
            );
        }
    };
    onMount(getGenres);
</script>

<h1>List of Genres</h1>
<table class="table table-striped">
    <thead>
        <tr>
            <td>GenreId</td>
            <td>Title</td>
            <td>ArtistId</td>
        </tr>
    </thead>
    <tbody>
        {#each genres as genre (genre.GenreId)}
            <tr>
                <td>{genre.GenreId}</td>
                <td>{genre.Name}</td>
                <td
                    ><a
                        href={`/admin/genres/${genre.GenreId}`}
                        class="btn btn-primary">Details</a
                    >
                    <button
                        class="btn btn-danger"
                        on:click={() => handleDelete(genre.GenreId)}
                        >Delete</button
                    >
                </td>
            </tr>
        {/each}
    </tbody>
</table>
