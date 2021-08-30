<script context="module">
    import { API_URL } from "$lib/env";

    export async function load({ page, fetch, session, context }) {
        if (page.params.id == 0) {
            return {
                props: {
                    genre: { Name: "" },
                },
            };
        }

        const url = `${API_URL}/genres/${page.params.id}`;
        const res = await fetch(url);

        if (res.ok) {
            const json = await res.json();
            return {
                props: {
                    genre: json.data,
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
    export let genre;

    let data = { Name: genre.Name };

    $: console.log("🚀 ~ file: [id].svelte ~ line 37 ~ data", data);

    let errors = { Name: [] };

    $: nameIsValid = data.Name.length == 0 || !data.Name.length;

    const getUpdatedGenre = async () => {
        const res = await fetch(`${API_URL}/genres/${genre.GenreId}`);
        const json = await res.json();
        genre = json.data;
    };

    const handleSubmit = async (event) => {
        try {
            const res = await fetch(`${API_URL}/genres/${genre.GenreId}`, {
                method: genre.GenreId ? "PUT" : "POST",
                body: JSON.stringify(data),
                headers: { "Content-Type": "application/json" },
            });
            console.log(
                "🚀 ~ file: [id].svelte ~ line 56 ~ handleSubmit ~ res",
                res
            );

            const json = await res.json();

            if (res.status == 200) {
                await getUpdatedGenre();
            }

            if (res.status == 422) {
                errors = jsjon.errors;
            }
        } catch (error) {
            console.log(
                "🚀 ~ file: [id].svelte ~ line 68 ~ handleSubmit ~ error",
                error
            );
        }
    };
</script>

<h1>{genre.Name}</h1>

<form class="form-floating mb-5" on:submit|preventDefault={handleSubmit}>
    <div class="form-floating mb-3">
        <input
            bind:value={data.Name}
            type="text"
            class="form-control"
            class:is-invalid={nameIsValid}
            id="NameInputField"
            placeholder="john doe"
        />
        <label for="NameInputField">Name</label>
    </div>
    <button type="submit" class="btn btn-primary">Submit</button>
    <a class="btn btn-dark" href={`/admin/genres`}>Back</a>
</form>

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
            aria-selected="true">Albums</button
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
            aria-selected="false">Tracks</button
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
        <h1>Display Rock Albums</h1>
        <div class="row gy-5">
            <div class="col">
                <div class="card mb-3">
                    <div class="card-body">
                        <h5 class="border-bottom my-2">Balls To The Wall</h5>
                        <h5 class="border-bottom my-2">Fast As a Shark</h5>
                        <h5 class="border-bottom my-2">Restless and Wild</h5>
                        <h5 class="border-bottom my-2">Princess of the Dawn</h5>
                        <h5 class="border-bottom my-2">Let's Get It Up</h5>
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
        <h1>Display Rock Tracks</h1>
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
</div>
