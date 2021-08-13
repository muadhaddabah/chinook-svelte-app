<script>
    import { API_URL } from "$lib/env";
    import { onMount } from "svelte";

    let media_types = [];
    const getMedia_types = async () => {
        const res = await fetch(`${API_URL}/media_types`);
        const json = await res.json();
        media_types = json.data;
    };
    onMount(getMedia_types);
</script>

<h1>List of Meadia Types</h1>

<table class="table table-striped">
    <thead>
        <tr>
            <td>MediaTypeId</td>
            <td>Name</td>
        </tr>
    </thead>
    <tbody>
        {#each media_types as media_type (media_type.MediaTypeId)}
            <tr>
                <td>{media_type.MediaTypeId}</td>
                <td>{media_type.Name}</td>
                <td
                    ><a
                        href={`/admin/media_types/${media_type.MediaTypeId}`}
                        class="btn btn-primary">Details</a
                    ></td
                >
            </tr>
        {/each}
    </tbody>
</table>
