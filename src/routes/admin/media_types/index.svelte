<script>
    import { API_URL } from "$lib/env";
    import { onMount } from "svelte";

    let media_types = [];
    const getMedia_types = async () => {
        const res = await fetch(`${API_URL}/media_types`);
        const json = await res.json();
        media_types = json.data;
    };

    const handleDelete = async (id) => {
        try {
            const res = await fetch(`${API_URL}/media_types/${id}`, {
                method: "DELETE",
            });
            if (res.status == 200) {
                await getMedia_types();
            }
            if (res.status == 419) {
                alert("Record can't be deleted");
            }
        } catch (error) {
            console.log(
                "🚀 ~ file: index.svelte ~ line 24 ~ handleDelete ~ error",
                error
            );
        }
    };
    onMount(getMedia_types);
</script>

<h1>List of Meadia Types</h1>

<table class="table table-striped">
    <thead>
        <tr>
            <td>MediaTypeId</td>
            <td>Name</td>
            <td>Actions</td>
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
                    >
                    <button
                        class="btn btn-danger"
                        on:click={() => handleDelete(media_type.MediaTypeId)}
                        >Delete</button
                    >
                </td>
            </tr>
        {/each}
    </tbody>
</table>
