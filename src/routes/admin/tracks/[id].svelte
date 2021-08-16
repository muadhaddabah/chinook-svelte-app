<script context="module">
    import { API_URL } from "$lib/env";

    export async function load({ page, fetch, session, context }) {
        if (page.params.id == 0) {
            return {
                props: {
                    track: {
                        Name: "",
                        Composer: "",
                        Milliseconds: "",
                        Bytes: "",
                        UnitPrice: "",
                    },
                },
            };
        }

        const url = `${API_URL}/tracks/${page.params.id}`;
        const res = await fetch(url);

        if (res.ok) {
            const json = await res.json();

            return {
                props: {
                    track: json.data,
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
    export let track;
    let data = {
        Name: track.Name,
        Composer: track.Composer,
        Milliseconds: track.Milliseconds,
        Bytes: track.Bytes,
        UnitPrice: track.UnitPrice,
    };

    $: console.log("🚀 ~ file: [id].svelte ~ line 48 ~ data", data);

    let errors = {
        Name: [],
        Composer: [],
        Milliseconds: [],
        Bytes: [],
        UnitPrice: [],
    };

    $: nameIsValid = data.Name.length == 0 || !data.Name.length;
    $: composerIsValid = data.Composer.length == 0 || !data.Composer.length;
    $: millisecondsIsValid =
        data.Milliseconds.length == 0 || !data.Milliseconds.length;
    $: bytesIsValid = data.Bytes.length == 0 || !data.Bytes.length;
    $: unitPriceIsValid = data.UnitPrice.length == 0 || !data.UnitPrice.length;

    const getUpdatedTrack = async () => {
        const res = await fetch(`${API_URL}/tracks/${track.TrackId}`);
        const json = await res.json();
        track = json.data;
    };

    const handleSubmit = async (event) => {
        try {
            const res = await fetch(`${API_URL}/tracks/${track.TrackId}`, {
                method: track.TrackId ? "PUT" : "POST",
                body: JSON.stringify(data),
                headers: { "Content-Type": "application/json" },
            });
            console.log(
                "🚀 ~ file: [id].svelte ~ line 43 ~ handleSubmit ~ res",
                res
            );

            const json = await res.json();

            if (res.status == 200) {
                await getUpdatedTrack();
            }

            if (res.status == 422) {
                errors = json.errors;
            }
        } catch (error) {
            console.log(
                "🚀 ~ file: [id].svelte ~ line 94 ~ handleSubmit ~ error",
                error
            );
        }
    };
</script>

<h1>{track.TrackId} {track.Name}</h1>

<form class="form-floating" on:submit|preventDefault={handleSubmit}>
    <div class="form-floating mb-3">
        <input
            bind:value={data.Name}
            type="text"
            class="form-control"
            class:is-invalid={nameIsValid}
            id="nameInputField"
            placeholder="john doe"
        />
        <label for="nameInputField">Name</label>
    </div>
</form>
<form class="form-floating" on:submit|preventDefault={handleSubmit}>
    <div class="form-floating mb-3">
        <input
            bind:value={data.Composer}
            type="text"
            class="form-control"
            class:is-invalid={composerIsValid}
            id="ComposerInputField"
        />
        <label for="ComposerInputField">Composer</label>
    </div>
</form>
<form class="form-floating" on:submit|preventDefault={handleSubmit}>
    <div class="form-floating mb-3">
        <input
            bind:value={data.Milliseconds}
            type="text"
            class="form-control"
            class:is-invalid={millisecondsIsValid}
            id="MillisecondsInputField"
        />
        <label for="MillisecondsInputField">Milliseconds</label>
    </div>
</form>
<form class="form-floating" on:submit|preventDefault={handleSubmit}>
    <div class="form-floating mb-3">
        <input
            bind:value={data.Bytes}
            type="text"
            class="form-control"
            class:is-invalid={bytesIsValid}
            id="BytesInputField"
        />
        <label for="BytesInputField">Bytes</label>
    </div>
</form>
<form class="form-floating" on:submit|preventDefault={handleSubmit}>
    <div class="form-floating mb-3">
        <input
            bind:value={data.UnitPrice}
            type="text"
            class="form-control"
            class:is-invalid={unitPriceIsValid}
            id="UnitPriceInputField"
        />
        <label for="UnitPriceInputField">UnitPrice</label>
    </div>
    <button type="submit" class="btn btn-primary">Submit</button>
    <a class="btn btn-dark" href={`/admin/tracks`}>Back</a>
</form>
