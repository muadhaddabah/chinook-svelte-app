<script context="module">
    import { API_URL } from "$lib/env";

    export async function load({ page, fetch, session, context }) {
        const url = `${API_URL}/employees/${page.params.id}`;
        const res = await fetch(url);

        if (res.ok) {
            const json = await res.json();
            console.log("🚀 ~ file: [id].svelte ~ line 10 ~ load ~ json", json);
            return {
                props: {
                    employee: json.data,
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
    export let employee;
</script>

<h1>{employee.FirstName}</h1>
