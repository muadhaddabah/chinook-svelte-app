<script context="module">
    import { API_URL } from "$lib/env";

    export async function load({ page, fetch, session, context }) {
        const url = `${API_URL}/invoices/${page.params.id}`;
        const res = await fetch(url);

        if (res.ok) {
            const json = await res.json();
            return {
                props: {
                    invoice: json.data,
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
    export let invoice;
</script>

<h1>{invoice.InvoiceId}</h1>

<table class="table table-striped">
    <thead>
        <tr>
            <td>InvoiceId</td>
            <td>CustomerId</td>
            <td>InvoiceDate</td>
            <td>BillingAddress</td>
            <td>BillingCity</td>
            <td>BillingState</td>
            <td>BillingCountry</td>
            <td>BillingPostalCode</td>
            <td>Total</td>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>{invoice.InvoiceId}</td>
            <td>{invoice.CustomerId}</td>
            <td>{invoice.InvoiceDate}</td>
            <td>{invoice.BillingAddress}</td>
            <td>{invoice.BillingCity}</td>
            <td>{invoice.BillingState}</td>
            <td>{invoice.BillingCountry}</td>
            <td>{invoice.BillingPostalCode}</td>
            <td>{invoice.Total}</td>
        </tr>
    </tbody>
</table>
