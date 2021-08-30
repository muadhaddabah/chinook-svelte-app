<script context="module">
    import { API_URL } from "$lib/env";

    export async function load({ page, fetch, session, context }) {
        if (page.params.id == 0) {
            return {
                props: {
                    customer: {
                        FirstName: "",
                        LastName: "",
                        Company: "",
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

        const url = `${API_URL}/customers/${page.params.id}/history`;
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

    let data = {
        FirstName: customer.FirstName || "",
        LastName: customer.LastName || "",
        Company: customer.Company || "",
        Address: customer.Address || "",
        City: customer.City || "",
        State: customer.State || "",
        Country: customer.Country || "",
        PostalCode: customer.PostalCode || "",
        Phone: customer.Phone || "",
        Fax: customer.Fax || "",
        Email: customer.Email || "",
    };
    $: console.log("🚀 ~ file: [id].svelte ~ line 59 ~ data", data);

    let errors = {
        FirstName: [],
        LastName: [],
        Company: [],
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
    $: companyIsValid = data.Company.length == 0 || !data.Company.length;
    $: addressIsValid = data.Address.length == 0 || !data.Address.length;
    $: cityIsValid = data.City.length == 0 || !data.City.length;
    $: stateIsValid = data.State.length == 0 || !data.State.length;
    $: countryIsValid = data.Country.length == 0 || !data.Country.length;
    $: postalCodeIsValid =
        data.PostalCode.length == 0 || !data.PostalCode.length;
    $: phoneIsValid = data.Phone.length == 0 || !data.Phone.length;
    $: faxIsValid = data.Fax.length == 0 || !data.Fax.length;
    $: emailIsValid = data.Email.length == 0 || !data.Email.length;

    const getUpdatedCustomer = async () => {
        const res = await fetch(`${API_URL}/customers/${customer.CustomerId}`);
        const json = await res.json();
        customer = json.data;
    };

    const handleSubmit = async (event) => {
        try {
            const res = await fetch(
                `${API_URL}/customers/${customer.CustomerId}`,
                {
                    method: customer.CustomerId ? "PUT" : "POST",
                    body: JSON.stringify(data),
                    headers: { "Content-Type": "application/json" },
                }
            );
            console.log(
                "🚀 ~ file: [id].svelte ~ line 93 ~ handleSubmit ~ res",
                res
            );

            const json = await res.json();

            if (res.status == 200) {
                await getUpdatedCustomer();
            }

            if (res.status == 422) {
                errors = json.errors;
            }
        } catch (error) {
            console.log(
                "🚀 ~ file: [id].svelte ~ line 106 ~ handleSubmit ~ error",
                error
            );
        }
    };
</script>

<h1>
    {customer.CustomerId}
    {customer.FirstName}
    {customer.LastName}
</h1>

<!-- bluecard -->
<div class="row">
    <div class="col-xl-3 col-md-6">
        <div class="widget widget-stats bg-blue">
            <div class="stats-info">
                <h4>Company</h4>
                <strong>{customer.Company}</strong><br />
                <strong>Email: {customer.Email}</strong>
            </div>
        </div>
    </div>
    <div class="col-xl-3 col-md-6">
        <div class="widget widget-stats bg-blue">
            <div class="stats-info">
                <h4>Phone</h4>
                <h5>{customer.Phone}</h5>
                <h5>Fax:{customer.Fax}</h5>
            </div>
        </div>
    </div>
    <div class="col-xl-3 col-md-6">
        <div class="widget widget-stats bg-blue">
            <div class="stats-info">
                <h4>TOTAL SPENT</h4>
                <h5>$16.83</h5>
            </div>
        </div>
    </div>
</div>

<!-- thingy start -->

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
            aria-selected="true">History</button
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
        <h1>Display Customer History</h1>
        <table class="table table-striped">
            <thead>
                <tr>
                    <td>Id</td>
                    <td>Date</td>
                    <td>Total</td>
                    <td># of items</td>
                    <td>Unit Price</td>
                    <td>Quantity</td>
                    <td>Name</td>
                </tr>
            </thead>
            <tbody>
                {#each customer.invoices as invoice (`${invoice.InvoiceId}_${invoice.InvoiceDate}`)}
                    <tr>
                        <th>{invoice.InvoiceId}</th>
                        <th>{invoice.InvoiceDate}</th>
                        <th>{invoice.Total}</th>
                        <th>{invoice.invoice_items.length}</th>
                        <th colspan="3">Invoice Items </th>
                    </tr>

                    {#each invoice.invoice_items as item, idx (`${item.InvoiceLineId}_${item.InvoiceId}`)}
                        <tr>
                            <td colspan="3" />
                            <td>{idx + 1} </td>
                            <td>{item.UnitPrice}</td>
                            <td>{item.Count}</td>
                            <td>{item.Name}</td>
                        </tr>
                    {/each}
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
            Edit {customer.FirstName}
            {customer.LastName}
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
                    bind:value={data.Company}
                    type="text"
                    class="form-control"
                    class:is-invalid={companyIsValid}
                />
                <label for="nameInputField">Company</label>
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
            </div>
            <button type="submit" class="btn btn-primary">Update</button>
        </form>
    </div>
</div>

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
