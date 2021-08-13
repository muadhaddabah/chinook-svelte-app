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

<h1>{employee.FirstName} {employee.LastName}</h1>

<table class="table table-striped">
    <thead>
        <tr>
            <td>EmployeeId</td>
            <td>First Name</td>
            <td>Last Name</td>
            <td>Title</td>
            <td>ReportsTo</td>
            <td>BirthDate</td>
            <td>HireDate</td>
            <td>Address</td>
            <td>City</td>
            <td>State</td>
            <td>Country</td>
            <td>PostalCode</td>
            <td>Phone</td>
            <td>Fax</td>
            <td>Email</td>
            <td>Actions</td>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>{employee.EmployeeId}</td>
            <td>{employee.FirstName}</td>
            <td>{employee.LastName}</td>
            <td>{employee.Title}</td>
            <td>{employee.ReportsTo}</td>
            <td>{employee.BirthDate}</td>
            <td>{employee.HireDate}</td>
            <td>{employee.Address}</td>
            <td>{employee.City}</td>
            <td>{employee.State}</td>
            <td>{employee.Country}</td>
            <td>{employee.PostalCode}</td>
            <td>{employee.Phone}</td>
            <td>{employee.Fax}</td>
            <td>{employee.Email}</td>
            <td
                ><a href={`/admin/employees`} class="btn btn-primary">Back</a
                ></td
            >
        </tr>
    </tbody>
</table>
