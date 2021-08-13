<script context="module">
    import { API_URL } from "$lib/env";

    export async function load({ page, fetch, session, context }) {
        const url = `${API_URL}/tracks/${page.params.id}`;
        const res = await fetch(url);

        if (res.ok) {
            const json = await res.json();

            return {
                props: {
                    track: json.data,
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
    export let track;
</script>

<h1>{track.TrackId}</h1>
<h1>{track.Name}</h1>

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
            <td><a href={`/admin/tracks`} class="btn btn-primary">Back</a></td>
        </tr>
    </tbody>
</table>
