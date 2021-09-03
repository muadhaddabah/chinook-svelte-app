<script context="module">
    import { API_URL } from "$lib/env";

    export async function load({ page, fetch, session, context }) {
        if (page.params.id == 0) {
            return {
                props: {
                    album: { Title: "", ArtistId: "" },
                },
            };
        }

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

    let data = { Title: album.Title };

    $: console.log("🚀 ~ file: [id].svelte ~ line 37 ~ data", data);

    let errors = { Title: [] };

    $: titleIsValid = data.Title.length == 0 || !data.Title.length;

    const getUpdatedAlbum = async () => {
        const res = await fetch(`${API_URL}/albums/${album.AlbumId}`);
        const json = await res.json();
        album = json.data;
    };

    const handleSubmit = async (event) => {
        try {
            const res = await fetch(`${API_URL}/albums/${album.AlbumId}`, {
                method: album.AlbumId ? "PUT" : "POST",
                body: JSON.stringify(data),
                headers: { "Content-Type": "application/json" },
            });
            console.log(
                "🚀 ~ file: [id].svelte ~ line 56 ~ handleSubmit ~ res",
                res
            );

            const json = await res.json();

            if (res.status == 200) {
                await getUpdatedAlbum();
            }

            if (res.status == 422) {
                errors = json.errors;
            }
        } catch (error) {
            console.log(
                "🚀 ~ file: [id].svelte ~ line 63 ~ handleSubmit ~ error",
                error
            );
        }
    };
    console.log(
        "🚀 ~ file: [id].svelte ~ line 73 ~ handleSubmit ~ handleSubmit",
        handleSubmit
    );
    console.log(
        "🚀 ~ file: [id].svelte ~ line 73 ~ handleSubmit ~ handleSubmit",
        handleSubmit
    );
</script>

<h1>{album.AlbumId}: {album.Title}</h1>

<!-- start -->
<ul class="nav nav-tabs mt-3" id="myTab" role="tablist">
    <li class="nav-item" role="presentation">
        <button
            class="nav-link active"
            id="edit-tab"
            data-bs-toggle="tab"
            data-bs-target="#edit"
            type="button"
            role="tab"
            aria-controls="edit"
            aria-selected="true">Tracks</button
        >
    </li>
    <li class="nav-item" role="presentation">
        <button
            class="nav-link "
            id="invoice-items-tab"
            data-bs-toggle="tab"
            data-bs-target="#invoice-items"
            type="button"
            role="tab"
            aria-controls="invoice-items"
            aria-selected="false">Edit</button
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
        <div class="row gy-5">
            <!--  -->
            <div class="col">
                <div class="card mb-3">
                    <div class="card-body">
                        <h5 class="card-title">
                            {album.Title}
                        </h5>
                        <ul class="list-group list-group-flush">
                            {#each album.Tracks as track (`${track.AlbumId}_${track.TrackId}`)}
                                <li class="list-group-item">
                                    {track.Name}
                                </li>
                            {/each}
                        </ul>
                    </div>
                </div>
            </div>
            <!-- <p>../</p> -->
        </div>
    </div>
    <div
        class="tab-pane fade"
        id="invoice-items"
        role="tabpanel"
        aria-labelledby="invoice-items-tab"
    >
        <form
            class="form-floating mt-4"
            on:submit|preventDefault={handleSubmit}
        >
            <div class="form-floating mb-3">
                <input
                    bind:value={data.Title}
                    type="text"
                    class="form-control"
                    class:is-invalid={titleIsValid}
                    id="titleInputField"
                    placeholder="Title"
                />
                <label for="titleInputField">Title</label>
            </div>
            <button type="submit" class="btn btn-primary">Submit</button>
        </form>
    </div>
</div>
<!-- end -->
