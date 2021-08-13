<script context="module">
    import { API_URL } from "$lib/env";

    export async function load({ page, fetch, session, context }) {
        const url = `${API_URL}/genres/${page.params.id}`;
        const res = await fetch(url);

        if (res.ok) {
            const json = await res.json();
            return {
                props: {
                    genre: json.data,
                },
            };
        }

        return {
            status: res.status,
            error: new Error(`Could not load ${url}`),
        };
    }
</script>

<script>
    export let genre;
</script>

<h1>{genre.Name}</h1>

<table class="table table-striped">
    <thead>
        <tr>
            <td>GenreId</td>
            <td>Title</td>
            <td>ArtistId</td>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>{genre.GenreId}</td>
            <td>{genre.Name}</td>
            <td><a href={`/admin/genres`} class="btn btn-primary">Back</a></td>
        </tr>
    </tbody>
</table>
