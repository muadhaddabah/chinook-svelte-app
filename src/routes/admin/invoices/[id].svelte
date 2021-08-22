<script context="module">
    import { API_URL } from "$lib/env";

    export async function load({ page, fetch, session, context }) {
        if (page.params.id == 0) {
            return {
                props: {
                    invoice: {
                        InvoiceDate: "",
                        BillingAddress: "",
                        BillingCity: "",
                        BillingState: "",
                        BillingCountry: "",
                        BillingPostalCode: "",
                        Total: "",
                    },
                },
            };
        }

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
    let data = {
        InvoiceDate: invoice.InvoiceDate,
        BillingAddress: invoice.BillingAddress,
        BillingCity: invoice.BillingCity,
        BillingState: invoice.BillingState,
        BillingCountry: invoice.BillingCountry,
        BillingPostalCode: invoice.BillingPostalCode,
        Total: invoice.Total,
    };
    console.log("🚀 ~ file: [id].svelte ~ line 51 ~ data", data);

    let errors = {
        InvoiceDate: [],
        BillingAddress: [],
        BillingCity: [],
        BillingState: [],
        BillingCountry: [],
        BillingPostalCode: [],
        Total: [],
    };

    $: invoiceDateIsValid =
        data.InvoiceDate.length == 0 || !data.InvoiceDate.length;
    $: billingAddressIsValid =
        data.BillingAddress.length == 0 || !data.BillingAddress.length;
    $: billingCityIsValid =
        data.BillingCity.length == 0 || !data.BillingCity.length;
    $: billingStateIsValid =
        data.BillingState.length == 0 || !data.BillingState.length;
    $: billingCountryIsValid =
        data.BillingCountry.length == 0 || !data.BillingCountry.length;
    $: billingPostalCodeIsValid =
        data.BillingPostalCode.length == 0 || !data.BillingPostalCode.length;
    $: totalIsValid = data.Total == 0 || !data.Total;
    $: console.log(data.Total.length);

    const getUpdatedInvoice = async () => {
        const res = await fetch(`${API_URL}/invoices/${invoice.InvoiceId}`);
        const json = await res.json();
        invoice = json.data;
    };

    const handleSubmit = async (event) => {
        try {
            const res = await fetch(
                `${API_URL}/invoices/${invoice.InvoiceId}`,
                {
                    method: invoice.InvoiceId ? "PUT" : "POST",
                    body: JSON.stringify(data),
                    headers: { "Content-Type": "application/json" },
                }
            );
            console.log(
                "🚀 ~ file: [id].svelte ~ line 92 ~ handleSubmit ~ res",
                res
            );

            const json = await res.json();

            if (res.status == 200) {
                await getUpdatedInvoice();
            }

            if (res.status == 422) {
                errors = json.errors;
            }
        } catch (error) {
            console.log(
                "🚀 ~ file: [id].svelte ~ line 104 ~ handleSubmit ~ error",
                error
            );
        }
    };
</script>

<h1>{invoice.InvoiceId}</h1>

<!-- {JSON.stringify(invoice, null, 2)} -->

<ul class="nav nav-tabs" id="myTab" role="tablist">
    <li class="nav-item" role="presentation">
        <button
            class="nav-link active"
            id="edit-tab"
            data-bs-toggle="tab"
            data-bs-target="#edit"
            type="button"
            role="tab"
            aria-controls="edit"
            aria-selected="true">Edit</button
        >
    </li>
    <li class="nav-item" role="presentation">
        <button
            class="nav-link"
            id="invoice-items-tab"
            data-bs-toggle="tab"
            data-bs-target="#invoice-items"
            type="button"
            role="tab"
            aria-controls="invoice-items"
            aria-selected="false">Invoice Items</button
        >
    </li>
</ul>
<div class="tab-content" id="myTabContent">
    <div
        class="tab-pane fade show active p-5"
        id="edit"
        role="tabpanel"
        aria-labelledby="edit-tab"
    >
        <form class="form-floating" on:submit|preventDefault={handleSubmit}>
            <div class="form-floating mb-3">
                <input
                    bind:value={data.InvoiceDate}
                    type="text"
                    class="form-control"
                    class:is-invalid={invoiceDateIsValid}
                    id="InvoiceDateInputField"
                />
                <label for="InvoiceDateInputField">Invoice Date</label>
            </div>
            <div class="form-floating mb-3">
                <input
                    bind:value={data.BillingAddress}
                    type="text"
                    class="form-control"
                    class:is-invalid={billingAddressIsValid}
                    id="BillingAddressInputField"
                />
                <label for="BillingAddressInputField">Billing Address</label>
            </div>
            <div class="form-floating mb-3">
                <input
                    bind:value={data.BillingCity}
                    type="text"
                    class="form-control"
                    class:is-invalid={billingCityIsValid}
                    id="BillingCityInputField"
                />
                <label for="BillingCityInputField">Billing City</label>
            </div>
            <div class="form-floating mb-3">
                <input
                    bind:value={data.BillingState}
                    type="text"
                    class="form-control"
                    class:is-invalid={billingStateIsValid}
                    id="BillingStateInputField"
                />
                <label for="BillingStateInputField">Billing State</label>
            </div>
            <div class="form-floating mb-3">
                <input
                    bind:value={data.BillingCountry}
                    type="text"
                    class="form-control"
                    class:is-invalid={billingCountryIsValid}
                    id="BillingCountryInputField"
                />
                <label for="BillingCountryInputField">Billing Country</label>
            </div>
            <div class="form-floating mb-3">
                <input
                    bind:value={data.BillingPostalCode}
                    type="text"
                    class="form-control"
                    class:is-invalid={billingPostalCodeIsValid}
                    id="BillingPostalCodeInputField"
                />
                <label for="BillingPostalCodeInputField"
                    >Billing PostalCode</label
                >
            </div>

            <div class="form-floating mb-3">
                <input
                    bind:value={data.Total}
                    type="text"
                    class="form-control"
                    class:is-invalid={totalIsValid}
                    id="TolInputField"
                />
                <label for="TotalInputField">Total</label>
            </div>
            <button type="submit" class="btn btn-primary">Submit</button>
            <a class="btn btn-dark" href={`/admin/artists`}>Back</a>
        </form>
    </div>
    <div
        class="tab-pane fade"
        id="invoice-items"
        role="tabpanel"
        aria-labelledby="invoice-items-tab"
    >
        ...
    </div>
</div>
