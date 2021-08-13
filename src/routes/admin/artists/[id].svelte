<script context="module">
    import { API_URL } from "$lib/env";

    export async function load({ page, fetch, session, context }) {
        const url = `${API_URL}/artists/${page.params.id}`;
        const res = await fetch(url);

        if (res.ok) {
            const json = await res.json();

            return {
                props: {
                    artist: json.data,
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
    export let artist;
</script>

<h1>{artist.Name}</h1>

<table class="table table-striped">
    <thead>
        <tr>
            <td>ArtistId</td>
            <td>Name</td>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>{artist.ArtistId}</td>
            <td>{artist.Name}</td>
            <td><a href={`/admin/artists`} class="btn btn-primary">Back</a></td>
        </tr>
    </tbody>
</table>
