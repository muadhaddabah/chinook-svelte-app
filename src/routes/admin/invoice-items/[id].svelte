<script context="module">
    import { API_URL } from "$lib/env";
    export async function load({ page, fetch, session, context }) {
        if (page.params.id == 0) {
            return {
                props: {
                    invoice_item: { UnitPrice: "", Quantity: "" },
                },
            };
        }

        const url = `${API_URL}/invoice_items/${page.params.id}`;
        const res = await fetch(url);

        if (res.ok) {
            const json = await res.json();
            return {
                props: {
                    invoice_item: json.data,
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
    export let invoice_item;
    let data = {
        UnitPrice: invoice_item.UnitPrice,
        Quantity: invoice_item.Quantity,
    };
    $: console.log("🚀 ~ file: [id].svelte ~ line 37 ~ data", data);

    let errors = { UnitPrice: [], Quantity: [] };

    $: unitPriceIsValid = data.UnitPrice == 0 || !data.UnitPrice;
    $: quantityIsValid = data.Quantity == 0 || !data.Quantity;

    const getUpdatedInvoiceItem = async () => {
        const res = await fetch(
            `${API_URL}/invoice_items/${invoice_item.InvoiceLineId}`
        );
        const json = await res.json();
        invoice_item = json.data;
    };

    const handleSubmit = async (event) => {
        try {
            const res = await fetch(
                `${API_URL}/invoice_items/${invoice_item.InvoiceLineId}`,
                {
                    method: invoice_item.InvoiceLineId ? "PUT" : "POST",
                    body: JSON.stringify(data),
                    headers: { "Content-Type": "application/json" },
                }
            );
            console.log(
                "🚀 ~ file: [id].svelte ~ line 53 ~ constgetUpdatedInvoiceItem= ~ res",
                res
            );

            const json = await res.json();

            if (res.status == 200) {
                await getUpdatedInvoiceItem();
            }

            if (res.status == 422) {
                errors = json.errors;
            }
        } catch (error) {
            console.log(
                "🚀 ~ file: [id].svelte ~ line 69 ~ constgetUpdatedInvoiceItem= ~ error",
                error
            );
        }
    };
</script>

<h1>{invoice_item.InvoiceLineId}</h1>

<table class="table table-striped">
    <thead>
        <tr>
            <td>InvoiceLineId</td>
            <td>InvoiceId</td>
            <td>TrackId</td>
            <td>UnitPrice</td>
            <td>Quantity</td>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>{invoice_item.InvoiceLineId}</td>
            <td>{invoice_item.InvoiceId}</td>
            <td>{invoice_item.TrackId}</td>
            <td>{invoice_item.UnitPrice}</td>
            <td>{invoice_item.Quantity}</td>
        </tr>
    </tbody>
</table>

<form class="form-floating" on:submit|preventDefault={handleSubmit}>
    <div class="form-floating mb-3">
        <input
            bind:value={data.UnitPrice}
            type="text"
            class="form-control"
            class:is-invalid={unitPriceIsValid}
        />
        <label for="UnitPriceInputField">UnitPrice</label>
    </div>
    <div class="form-floating mb-3">
        <input
            bind:value={data.Quantity}
            type="text"
            class="form-control"
            class:is-invalid={quantityIsValid}
        />
        <label for="QuantityInputField">Quantity</label>
    </div>
    <button type="submit" class="btn btn-primary">Submit</button>
    <a class="btn btn-dark" href={`/admin/invoice-items`}>Back</a>
</form>
