<script context="module">
    import { API_URL } from "$lib/env";

    export async function load({ page, fetch, session, context }) {
        if (page.params.id == 0) {
            return {
                props: {
                    customer: {
                        FirstName: "",
                        LastName: "",
                        Title: "",
                        BirthDate: "",
                        HireDate: "",
                        Address: "",
                        City: "",
                        State: "",
                        Country: "",
                        PostalCode: "",
                        Phone: "",
                        Fax: "",
                        Email: "",
                    },
                },
            };
        }

        const url = `${API_URL}/employees/${page.params.id}/cust`;
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
    export let employee = [];

    let data = {
        EmployeeId: employee.EmployeeId || "",
        FirstName: employee.FirstName || "",
        LastName: employee.LastName || "",
        Title: employee.Title || "",
        BirthDate: employee.BirthDate || "",
        HireDate: employee.HireDate || "",
        Address: employee.Address || "",
        City: employee.City || "",
        State: employee.State || "",
        Country: employee.Country || "",
        PostalCode: employee.PostalCode || "",
        Phone: employee.Phone || "",
        Fax: employee.Fax || "",
        Email: employee.Email || "",
    };
    console.log("🚀 ~ file: [id].svelte ~ line 65 ~ data", data);

    let errors = {
        FirstName: [],
        LastName: [],
        Title: [],
        BirthDate: [],
        HireDate: [],
        Address: [],
        City: [],
        State: [],
        Country: [],
        PostalCode: [],
        Phone: [],
        Fax: [],
        Email: [],
    };

    $: firstNameIsValid = data.FirstName.length == 0 || !data.FirstName.length;
    $: lastNameIsValid = data.LastName.length == 0 || !data.LastName.length;
    $: titleIsValid = data.Title.length == 0 || !data.Title.length;
    $: birthDateIsValid = data.BirthDate.length == 0 || !data.BirthDate.length;
    $: hireDateIsValid = data.HireDate.length == 0 || !data.HireDate.length;
    $: addressIsValid = data.Address.length == 0 || !data.Address.length;
    $: cityIsValid = data.City.length == 0 || !data.City.length;
    $: stateIsValid = data.State.length == 0 || !data.State.length;
    $: countryIsValid = data.Country.length == 0 || !data.Country.length;
    $: postalCodeIsValid =
        data.PostalCode.length == 0 || !data.PostalCode.length;
    $: phoneIsValid = data.Phone.length == 0 || !data.Phone.length;
    $: faxIsValid = data.Fax.length == 0 || !data.Fax.length;
    $: emailIsValid = data.Email.length == 0 || !data.Email.length;

    const getUpdatedEmployee = async () => {
        const res = await fetch(`${API_URL}/employees/${employee.EmployeeId}`);
        const json = await res.json();
        employee = res.data;
    };

    const handleSubmit = async (event) => {
        try {
            const res = await fetch(
                `${API_URL}/employees/${employee.EmployeeId}`,
                {
                    method: employee.EmployeeId ? "PUT" : "POST",
                    body: JSON.stringify(data),
                    headers: { "Content-Type": "application/json" },
                }
            );
            console.log(
                "🚀 ~ file: [id].svelte ~ line 114 ~ handleSubmit ~ res",
                res
            );

            const json = await res.json();

            if (res.status == 200) {
                await getUpdatedEmployee();
            }

            if (res.status == 422) {
                errors = json.errors;
            }
        } catch (error) {
            console.log(
                "🚀 ~ file: [id].svelte ~ line 129 ~ handleSubmit ~ error",
                error
            );
        }
    };
</script>

<h1>{employee.FirstName} {employee.LastName}</h1>

<div class="row">
    <div class="col-xl-3 col-md-6">
        <div class="widget widget-stats bg-blue">
            <div class="stats-info">
                <h4>{employee.Title}</h4>
                <h5>Phone: {employee.Phone}</h5>
                <h5>Email: {employee.Email}</h5>
            </div>
        </div>
    </div>
    <div class="col-xl-3 col-md-6">
        <div class="widget widget-stats bg-blue">
            <div class="stats-info">
                <h4>TOTAL CUSTOMERS</h4>
                <h5>need to be added</h5>
            </div>
        </div>
    </div>
</div>

<!-- start -->

<ul class="nav nav-tabs mt-3" id="myTab" role="tablist">
    <li class="nav-item" role="presentation">
        <button
            class="nav-link active"
            id="edit-tab"
            data-bs-toggle="tab"
            data-bs-target="#edit"
            type="button"
            role="tab"
            aria-controls="edit"
            aria-selected="true">Customers</button
        >
    </li>
    <li class="nav-item" role="presentation">
        <button
            class="nav-link "
            id="invoice-items-tab"
            data-bs-toggle="tab"
            data-bs-target="#invoice-items"
            type="button"
            role="tab"
            aria-controls="invoice-items"
            aria-selected="false"
            >Edit
        </button>
    </li>
</ul>
<!-- table -->
<div class="tab-content " id="myTabContent">
    <div
        class="tab-pane fade show active p-5"
        id="edit"
        role="tabpanel"
        aria-labelledby="edit-tab"
    >
        <h1>{employee.FirstName} {employee.LastName} Customers</h1>
        <table class="table table-striped">
            <thead>
                <tr>
                    <td>CustomerId</td>
                    <td>FirstName</td>
                    <td>LastName</td>
                    <td>Phone</td>
                    <td>SuppportRepId</td>
                </tr>
            </thead>
            <tbody>
                {#each employee.Customers as customer (`${customer.CustomerId}_${customer.FirstName}`)}
                    <tr>
                        <td>{customer.CustomerId}</td>
                        <td>{customer.FirstName}</td>
                        <td>{customer.LastName}</td>
                        <td>{customer.Phone}</td>
                        <td>{customer.SupportRepId}</td>
                    </tr>
                {/each}
            </tbody>
        </table>
    </div>
    <div
        class="tab-pane fade p-5"
        id="invoice-items"
        role="tabpanel"
        aria-labelledby="invoice-items-tab"
    >
        <h1>
            Edit {employee.FirstName}
            {employee.LastName}
        </h1>
        <!-- Form -->
        <form class="form-floating" on:submit|preventDefault={handleSubmit}>
            <div class="form-floating mb-3">
                <input
                    bind:value={data.FirstName}
                    type="text"
                    class="form-control"
                    class:is-invalid={firstNameIsValid}
                    id="firstNameInputField"
                    placeholder="John"
                />
                <label for="nameInputField">First Name</label>
            </div>
            <div class="form-floating mb-3">
                <input
                    bind:value={data.LastName}
                    type="text"
                    class="form-control"
                    class:is-invalid={lastNameIsValid}
                    id="LastNameInputField"
                    placeholder="doe"
                />
                <label for="nameInputField">Last Name</label>
            </div>
            <div class="form-floating mb-3">
                <input
                    bind:value={data.Title}
                    type="text"
                    class="form-control"
                    class:is-invalid={titleIsValid}
                />
                <label for="nameInputField">Title</label>
            </div>
            <div class="form-floating mb-3">
                <input
                    bind:value={data.BirthDate}
                    type="text"
                    class="form-control"
                    class:is-invalid={birthDateIsValid}
                />
                <label for="nameInputField">BirthDate</label>
            </div>
            <div class="form-floating mb-3">
                <input
                    bind:value={data.HireDate}
                    type="text"
                    class="form-control"
                    class:is-invalid={hireDateIsValid}
                />
                <label for="nameInputField">HireDate</label>
            </div>

            <div class="form-floating mb-3">
                <input
                    bind:value={data.Address}
                    type="text"
                    class="form-control"
                    class:is-invalid={addressIsValid}
                />
                <label for="nameInputField">Address</label>
            </div>

            <div class="form-floating mb-3">
                <input
                    bind:value={data.City}
                    type="text"
                    class="form-control"
                    class:is-invalid={cityIsValid}
                />
                <label for="nameInputField">City</label>
            </div>
            <div class="form-floating mb-3">
                <input
                    bind:value={data.State}
                    type="text"
                    class="form-control"
                    class:is-invalid={stateIsValid}
                />
                <label for="nameInputField">State</label>
            </div>
            <div class="form-floating mb-3">
                <input
                    bind:value={data.Country}
                    type="text"
                    class="form-control"
                    class:is-invalid={countryIsValid}
                />
                <label for="nameInputField">Country</label>
            </div>
            <div class="form-floating mb-3">
                <input
                    bind:value={data.PostalCode}
                    type="text"
                    class="form-control"
                    class:is-invalid={postalCodeIsValid}
                />
                <label for="nameInputField">Postal Code</label>
            </div>
            <div class="form-floating mb-3">
                <input
                    bind:value={data.Phone}
                    type="text"
                    class="form-control"
                    class:is-invalid={phoneIsValid}
                />
                <label for="nameInputField">Phone </label>
            </div>
            <div class="form-floating mb-3">
                <input
                    bind:value={data.Fax}
                    type="text"
                    class="form-control"
                    class:is-invalid={faxIsValid}
                />
                <label for="nameInputField">Fax </label>
            </div>
            <div class="form-floating mb-3">
                <input
                    bind:value={data.Email}
                    type="text"
                    class="form-control"
                    class:is-invalid={emailIsValid}
                />
                <label for="nameInputField">Email </label>

                <button type="submit" class="btn btn-primary mt-2"
                    >Update</button
                >
            </div>
        </form>
    </div>
</div>

<!-- styles -->
<style>
    .widget.widget-stats {
        position: relative;
        color: #fff;
        padding: 0.9375rem;
        border-radius: 4px;
    }
    .widget {
        overflow: hidden;
        margin-bottom: 20px;
        background: #fff;
        color: inherit;
        padding: 0;
    }
    .bg-blue {
        background-color: #348fe2 !important;
    }
</style>
