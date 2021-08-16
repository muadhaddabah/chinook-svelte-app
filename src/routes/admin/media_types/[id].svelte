<script context="module">
    import { API_URL } from "$lib/env";

    export async function load({ page, fetch, session, context }) {
        if (page.params.id == 0) {
            return {
                props: {
                    media_type: { Name: "" },
                },
            };
        }

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
    let data = { Name: media_type.Name };

    $: console.log("🚀 ~ file: [id].svelte ~ line 31 ~ data", data);
    let errors = { Name: [] };

    $: nameIsValid = data.Name.length == 0 || !data.Name.length;

    const getUpdatedMedia_type = async () => {
        const res = await fetch(
            `${API_URL}/media_types/${media_type.MediaTypeId}`
        );
        const json = await res.json();
        media_type = json.data;
    };

    const handleSubmit = async (event) => {
        try {
            const res = await fetch(
                `${API_URL}/media_types/${media_type.MediaTypeId}`,
                {
                    method: media_type.MediaTypeId ? "PUT" : "POST",
                    body: JSON.stringify(data),
                    headers: { "Content-Type": "application/json" },
                }
            );
            console.log(
                "🚀 ~ file: [id].svelte ~ line 60 ~ handleSubmit ~ res",
                res
            );

            const json = await res.json();

            if (res.status == 200) {
                await getUpdatedMedia_type();
            }

            if (res.status == 422) {
                errors = json.errors;
            }
        } catch (error) {
            console.log(
                "🚀 ~ file: [id].svelte ~ line 46 ~ handleSubmit ~ error",
                error
            );
        }
    };
</script>

<h1>{media_type.MediaTypeId} {media_type.Name}</h1>

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
        </tr>
    </tbody>
</table>

<form class="form-floating" on:submit|preventDefault={handleSubmit}>
    <div class="form-floating mb-3">
        <input
            bind:value={data.Name}
            type="text"
            class="form-control"
            class:is-invalid={nameIsValid}
            id="nameInputField"
        />
        <label for="nameInputField">Name</label>
    </div>
    <button type="submit" class="btn btn-primary">Submit</button>
    <a class="btn btn-dark" href={`/admin/media_types`}>Back</a>
</form>
