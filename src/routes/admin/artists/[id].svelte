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

    // $: console.log("🚀 ~ file: [id].svelte ~ line 40 ~ data2", data2);
    $: console.log("🚀 ~ file: [id].svelte ~ line 31 ~ data", data);
    let errors = { Name: [] };

    // $: nameIsValid = data.Name.length == 0 || !data.Name.length;
    $: saveIsVisible = true;
    const getUpdatedArtist = async () => {
        const res = await fetch(`${API_URL}/albums/${artist.ArtistId}`);
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

<h1>{artist.Name}</h1>

<h1>
    <span
        contenteditable
        on:input={(event) => handleChange(event, artist.Name)}
    >
        {artist.Name}
        <!-- {artist.Album.Title} -->
    </span>
    <button
        class="btn btn-primary float-right "
        class:invisible={saveIsVisible}
        on:click|preventDefault={handleSubmit}>Save</button
    >
</h1>

<ul class="nav nav-tabs" id="myTab" role="tablist">
    <li class="nav-item" role="presentation">
        <button
            class="nav-link active"
            id="edit-tab"
            data-bs-toggle="tab"
            data-bs-target="#edit"
            type="button"
            role="tab"
            aria-controls="edit"
            aria-selected="true">View</button
        >
    </li>
    <li class="nav-item" role="presentation">
        <button
            class="nav-link"
            id="invoice-items-tab"
            data-bs-toggle="tab"
            data-bs-target="#invoice-items"
            type="button"
            role="tab"
            aria-controls="invoice-items"
            aria-selected="false">Invoice Items</button
        >
    </li>
</ul>
<div class="tab-content" id="myTabContent">
    <div
        class="tab-pane fade show active p-5"
        id="edit"
        role="tabpanel"
        aria-labelledby="edit-tab"
    >
        <div class="card text-center" style="width: 18rem;">
            <div class="card-body">
                <span>
                    <input
                        value={data.Title}
                        class="card-title"
                        id="albumTitle"
                    />{data.Title}
                </span>
                <ul class="card-text">
                    <li>
                        With supporting text below as a natural lead-in to
                        additional content.
                    </li>
                </ul>
            </div>
        </div>
    </div>
    <div
        class="tab-pane fade"
        id="invoice-items"
        role="tabpanel"
        aria-labelledby="invoice-items-tab"
    >
        ...
    </div>
</div>

<div class="card text-center" style="width: 18rem;">
    <div class="card-body">
        <input bind:value={data.Title} class="card-title" />
        <p class="card-text">
            With supporting text below as a natural lead-in to additional
            content.
        </p>
    </div>
</div>
