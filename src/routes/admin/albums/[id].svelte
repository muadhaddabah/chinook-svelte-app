<script context="module">
    import { API_URL } from "$lib/env";

    export async function load({ page, fetch, session, context }) {
        const url = `${API_URL}/albums/${page.params.id}`;
        const res = await fetch(url);

        if (res.ok) {
            const json = await res.json();
            return {
                props: {
                    album: json.data,
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
    export let album;
</script>

<h1>{album.Title}</h1>

<table class="table table-striped">
    <thead>
        <tr>
            <td>AlbumId</td>
            <td>Title</td>
            <td>ArtistId</td>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>{album.AlbumId}</td>
            <td>{album.Title}</td>
            <td>{album.ArtistId}</td>
            <td><a href={`/admin/albums`} class="btn btn-primary">Back</a></td>
        </tr>
    </tbody>
</table>
