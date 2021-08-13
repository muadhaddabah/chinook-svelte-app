<script>
    import { API_URL } from "$lib/env";
    import { onMount } from "svelte";

    let genres = [];
    const getGenres = async () => {
        const res = await fetch(`${API_URL}/genres`);
        const json = await res.json();
        genres = json.data;
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
                    ></td
                >
            </tr>
        {/each}
    </tbody>
</table>
