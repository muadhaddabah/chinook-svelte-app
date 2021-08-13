<script context="module">
    import { API_URL } from "$lib/env";

    export async function load({ page, fetch, session, context }) {
        const url = `${API_URL}/customers/${page.params.id}`;
        const res = await fetch(url);

        if (res.ok) {
            const json = await res.json();
            return {
                props: {
                    customer: json.data,
                },
            };
        }
        return {
            status: res.status,
            error: new Error(`couldn't load ${url}`),
        };
    }
</script>

<script>
    export let customer;
</script>

<h1>
    {customer.CustomerId}
    {customer.FirstName}
    {customer.LastName}
</h1>

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
                ><a href={`/admin/customers`} class="btn btn-primary">Back</a
                ></td
            >
        </tr>
    </tbody>
</table>
