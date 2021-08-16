<script context="module">
    import { API_URL } from "$lib/env";

    export async function load({ page, fetch, session, context }) {
        if (page.params.id == 0) {
            return {
                props: {
                    artist: { Name: "" },
                },
            };
        }
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
    let data = { Name: artist.Name };

    $: console.log("🚀 ~ file: [id].svelte ~ line 31 ~ data", data);
    let errors = { Name: [] };

    $: nameIsValid = data.Name.length == 0 || !data.Name.length;
    $: saveIsVisible = true;
    const getUpdatedArtist = async () => {
        const res = await fetch(`${API_URL}/artists/${artist.ArtistId}`);
        const json = await res.json();
        artist = json.data;
    };

    const handleSubmit = async (event) => {
        try {
            const res = await fetch(`${API_URL}/artists/${artist.ArtistId}`, {
                method: artist.ArtistId ? "PUT" : "POST",
                body: JSON.stringify(data),
                headers: { "Content-Type": "application/json" },
            });
            console.log(
                "🚀 ~ file: [id].svelte ~ line 43 ~ handleSubmit ~ res",
                res
            );

            const json = await res.json();

            if (res.status == 200) {
                await getUpdatedArtist();
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

    const handleChange = async (event, artistName) => {
        console.log(
            "🚀 ~ file: [id].svelte ~ line 77 ~ handleChange ~ event",
            event.target.innerText
        );

        data.Name = event.target.innerText;
        saveIsVisible = !(event.target.innerText != artistName);
    };
</script>

<h1>
    <span
        contenteditable
        on:input={(event) => handleChange(event, artist.Name)}
    >
        {artist.Name}
    </span>
    <button
        class="btn btn-primary float-right "
        class:invisible={saveIsVisible}
        on:click|preventDefault={handleSubmit}>Save</button
    >
</h1>
