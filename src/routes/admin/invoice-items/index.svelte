<script>
    import { API_URL } from "$lib/env";
    import { onMount } from "svelte";

    let invoice_items = [];
    const getInvoice_items = async () => {
        const res = await fetch(`${API_URL}/invoice_items`);
        const json = await res.json();
        invoice_items = json.data;
        console.log(
            "🚀 ~ file: index.svelte ~ line 8 ~ getInvoice_items ~ invoice_items",
            invoice_items
        );
    };
    onMount(getInvoice_items);
</script>

<h1>List of Invoice Items</h1>
<table class="table table-striped">
    <thead>
        <tr>
            <td>InvoiceLineId</td>
            <td>InvoiceId</td>
            <td>TrackId</td>
            <td>UnitPrice</td>
            <td>Quantity</td>
            <td>Actions</td>
        </tr>
    </thead>
    <tbody>
        {#each invoice_items as invoice_item (invoice_item.InvoiceLineId)}
            <tr>
                <td>{invoice_item.InvoiceLineId}</td>
                <td>{invoice_item.InvoiceId}</td>
                <td>{invoice_item.TrackId}</td>
                <td>{invoice_item.UnitPrice}</td>
                <td>{invoice_item.Quantity}</td>
                <td
                    ><a
                        href={`/admin/invoice_items/${invoice_item.InvoiceLineId}`}
                        class="btn btn-primary">Details</a
                    ></td
                >
            </tr>
        {/each}
    </tbody>
</table>
