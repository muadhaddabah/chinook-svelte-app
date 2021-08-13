<script context="module">
    import { API_URL } from "$lib/env";

    export async function load({ page, fetch, session, context }) {
        const url = `${API_URL}/playlists/${page.params.id}`;
        const res = await fetch(url);

        if (res.ok) {
            const json = await res.json();

            return {
                props: {
                    playlist: json.data,
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
    export let playlist;
</script>

<h1>{playlist.Name}</h1>

<table class="table table-striped">
    <thead>
        <tr>
            <td>PlaylistId</td>
            <td>Name</td>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>{playlist.PlaylistId}</td>
            <td>{playlist.Name}</td>
            <td
                ><a href={`/admin/playlists`} class="btn btn-primary">Back</a
                ></td
            >
        </tr>
    </tbody>
</table>
