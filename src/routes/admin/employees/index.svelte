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
            <td>First Name</td>
            <td>Last Name</td>
            <td>Actions</td>
        </tr>
    </thead>
    <tbody>
        {#each employees as employee (employee.EmployeeId)}
            <tr>
                <td>{employee.FirstName}</td>
                <td>{employee.LastName}</td>
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
