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

<h1>{genre.GenreId} {genre.Name}</h1>

<form class="form-floating" on:submit|preventDefault={handleSubmit}>
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
