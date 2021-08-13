<script context="module">
    import { API_URL } from "$lib/env";
    export async function load({ page, fetch, session, context }) {
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
            <td
                ><a href={`/admin/invoice_items`} class="btn btn-primary"
                    >Back</a
                ></td
            >
        </tr>
    </tbody>
</table>
