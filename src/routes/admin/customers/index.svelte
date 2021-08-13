<script>
    import { API_URL } from "$lib/env";
    import { onMount } from "svelte";

    let customers = [];
    const getCustomers = async () => {
        const res = await fetch(`${API_URL}/customers`);
        const json = await res.json();
        customers = json.data;
    };
    onMount(getCustomers);
</script>

<h1>List of Customers (Table)</h1>
<table class="table table-striped">
    <thead>
        <tr>
            <td>Customer Id</td>
            <td>First Name</td>
            <td>Last Name</td>
            <td>Company</td>
            <td>Address</td>
            <td>City</td>
            <td>State</td>
            <td>Country</td>
            <td>PostalCode</td>
            <td>Phone</td>
            <td>Fax</td>
            <td>Email</td>
            <td>SupportRepId</td>
            <td>Actions</td>
        </tr>
    </thead>
    <tbody>
        {#each customers as customer (customer.CustomerId)}
            <tr>
                <td>{customer.CustomerId}</td>
                <td>{customer.FirstName}</td>
                <td>{customer.LastName}</td>
                <td>{customer.Company}</td>
                <td>{customer.Address}</td>
                <td>{customer.City}</td>
                <td>{customer.State}</td>
                <td>{customer.Country}</td>
                <td>{customer.PostalCode}</td>
                <td>{customer.Phone}</td>
                <td>{customer.Fax}</td>
                <td>{customer.Email}</td>
                <td>{customer.SupportRepId}</td>
                <td
                    ><a
                        href={`/admin/customers/${customer.CustomerId}`}
                        class="btn btn-primary">Details</a
                    ></td
                >
            </tr>
        {/each}
    </tbody>
</table>
