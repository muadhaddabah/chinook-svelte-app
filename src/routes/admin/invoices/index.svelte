<script>
    import { API_URL } from "$lib/env";
    import { onMount } from "svelte";
    let invoices = [];
    const getInvoices = async () => {
        const res = await fetch(`${API_URL}/invoices`);
        const json = await res.json();
        invoices = json.data;
    };

    const handleDelete = async (id) => {
        try {
            const res = await fetch(`${API_URL}/invoices/${id}`, {
                method: "DELETE",
            });
            if (res.status == 200) {
                await getInvoices();
            }
            if (res.status == 419) {
                alert("Record Can't be deleted");
            }
        } catch (error) {
            console.log(
                "🚀 ~ file: index.svelte ~ line 23 ~ handleDelete ~ error",
                error
            );
        }
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
                    >
                    <button
                        class="btn btn-danger"
                        on:click={() => handleDelete(invoice.InvoiceId)}
                        >Delete</button
                    >
                </td>
            </tr>
        {/each}
    </tbody>
</table>
