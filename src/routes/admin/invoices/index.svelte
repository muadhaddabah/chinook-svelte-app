<script>
    import { API_URL } from "$lib/env";
    import { onMount } from "svelte";
    let invoices = [];
    const getInvoices = async () => {
        const res = await fetch(`${API_URL}/invoices`);
        const json = await res.json();
        invoices = json.data;
    };
    onMount(getInvoices);
</script>

<h1>List of Invoices</h1>
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
        {#each invoices as invoice (invoice.InvoiceId)}
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
                <td
                    ><a
                        href={`/admin/invoices/${invoice.InvoiceId}`}
                        class="btn btn-primary">Details</a
                    ></td
                >
            </tr>
        {/each}
    </tbody>
</table>
