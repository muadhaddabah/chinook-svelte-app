<script>
    import { API_URL } from "$lib/env";
    import { onMount } from "svelte";

    let employees = [];
    const getEmployees = async () => {
        const res = await fetch(`${API_URL}/employees`);
        const json = await res.json();
        employees = json.data;
        console.log(
            "🚀 ~ file: index.svelte ~ line 8 ~ getEmployees ~ employees",
            employees
        );
    };
    onMount(getEmployees);
</script>

<h1>List of Employees (Table)</h1>
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
        {#each employees as employee (employee.EmployeeId)}
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
                    ><a
                        href={`/admin/employees/${employee.EmployeeId}`}
                        class="btn btn-primary">Details</a
                    ></td
                >
            </tr>
        {/each}
    </tbody>
</table>
