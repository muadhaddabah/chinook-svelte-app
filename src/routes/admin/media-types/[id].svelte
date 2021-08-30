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

        const url = `${API_URL}/media_types/${page.params.id}/trac`;
        const res = await fetch(url);

        if (res.ok) {
            const json = await res.json();

            console.log(json);
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
            `${API_URL}/media-types/${media_type.MediaTypeId}`
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

<!-- <table class="table table-striped">
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
</table> -->

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
    <button type="submit" class="btn btn-primary ">Submit</button>
    <a class="btn btn-dark" href={`/admin/media-types`}>Back</a>
</form>

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
            aria-selected="false">Other</button
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
        <h1>Display {media_type.Name} Tracks</h1>
        <!-- {JSON.stringify(media_type, null, 2)} -->
        <!-- start -->
        <div class="row gy-5">
            <ul class="list-group list-group-flush">
                {#each media_type.tracks as track (`${track.TrackId}_${track.Name}`)}
                    <li class="list-group-item">
                        {track.Name}
                    </li>
                {/each}
            </ul>
        </div>
        <!-- end -->
        <div class="row gy-5">
            <div class="col">
                <div class="card mb-3">
                    <div class="card-body">
                        <h5 class="border-bottom my-2">
                            I Was Made For Lovin You
                        </h5>
                        <h5 class="border-bottom my-2">Rock Song</h5>
                        <h5 class="border-bottom my-2">No One Like You</h5>
                        <h5 class="border-bottom my-2">Livin On a Prayer</h5>
                        <h5 class="border-bottom my-2">
                            Rock You Like a Hurricane
                        </h5>
                        <h5 class="my-2">Put The Finger On You</h5>
                    </div>
                </div>
            </div>
        </div>
    </div>
    <div
        class="tab-pane fade p-5"
        id="invoice-items"
        role="tabpanel"
        aria-labelledby="invoice-items-tab"
    >
        <h1>How</h1>
        <div class="row gy-5">
            ...
            <!-- <div class="col">
                <div class="card mb-3">
                    <div class="card-body">
                        <h5 class="border-bottom my-2">
                            I Was Made For Lovin You
                        </h5>
                        <h5 class="border-bottom my-2">Rock Song</h5>
                        <h5 class="border-bottom my-2">No One Like You</h5>
                        <h5 class="border-bottom my-2">Livin On a Prayer</h5>
                        <h5 class="border-bottom my-2">
                            Rock You Like a Hurricane
                        </h5>
                        <h5 class="my-2">Put The Finger On You</h5>
                    </div>
                </div>
            </div> -->
        </div>
    </div>
</div>
