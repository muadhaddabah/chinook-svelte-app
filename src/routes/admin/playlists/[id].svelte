<script context="module">
    import { API_URL } from "$lib/env";

    export async function load({ page, fetch, session, context }) {
        if (page.params.id == 0) {
            return {
                props: {
                    playlist: { Name: "" },
                },
            };
        }

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
    let data = { Name: playlist.Name };
    $: console.log("🚀 ~ file: [id].svelte ~ line 37 ~ data", data);

    let errors = { Name: [] };
    $: nameIsValid = data.Name.length == 0 || !data.Name.length;

    const getUpdatedPlaylist = async () => {
        const res = await fetch(`${API_URL}/playlists/${playlist.PlaylistId}`);
        const json = await res.json();
        playlist = json.data;
    };

    const handleSubmit = async (event) => {
        try {
            const res = await fetch(
                `${API_URL}/playlists/${playlist.PlaylistId}`,
                {
                    method: playlist.PlaylistId ? "PUT" : "POST",
                    body: JSON.stringify(data),
                    headers: { "Content-Type": "application/json" },
                }
            );
            console.log(
                "🚀 ~ file: [id].svelte ~ line 43 ~ handleSubmit ~ res",
                res
            );

            const json = await res.json();

            if (res.status == 200) {
                await getUpdatedPlaylist();
            }

            if (res.status == 422) {
                errors = json.errors;
            }
        } catch (error) {
            console.log(
                "🚀 ~ file: [id].svelte ~ line 69 ~ onsthandleSubmit= ~ error",
                error
            );
        }
    };
</script>

<h1>{playlist.Name}</h1>

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
    <a class="btn btn-dark" href={`/admin/playlists`}>Back</a>
</form>
