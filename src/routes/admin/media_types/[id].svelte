<script context="module">
    import { API_URL } from "$lib/env";

    export async function load({ page, fetch, session, context }) {
        const url = `${API_URL}/media_types/${page.params.id}`;
        const res = await fetch(url);

        if (res.ok) {
            const json = await res.json();

            return {
                props: {
                    media_type: json.data,
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
    export let media_type;
</script>

<h1>{media_type.Name}</h1>

<table class="table table-striped">
    <thead>
        <tr>
            <td>MediaTypeId</td>
            <td>Name</td>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>{media_type.MediaTypeId}</td>
            <td>{media_type.Name}</td>
            <td
                ><a href={`/admin/media_types`} class="btn btn-primary">Back</a
                ></td
            >
        </tr>
    </tbody>
</table>
