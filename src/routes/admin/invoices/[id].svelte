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

    // cannot read 'length'
    // $: billingStateIsValid =
    //     data.BillingState.length == 0 || !data.BillingState.length;
    $: billingCountryIsValid =
        data.BillingCountry.length == 0 || !data.BillingCountry.length;

    $: billingPostalCodeIsValid =
        data.BillingPostalCode.length == 0 || !data.BillingPostalCode.length;
    $: totalIsValid = data.Total.length == 0 || !data.Total.length;

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
        </tr>
    </tbody>
</table>

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
</form>

<form class="form-floating" on:submit|preventDefault={handleSubmit}>
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
</form>
<form class="form-floating" on:submit|preventDefault={handleSubmit}>
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
</form>
<form class="form-floating" on:submit|preventDefault={handleSubmit}>
    <div class="form-floating mb-3">
        <input
            bind:value={data.BillingState}
            type="text"
            class="form-control"
            id="BillingStateInputField"
        />
        <label for="BillingStateInputField">Billing State</label>
    </div>
</form>
<form class="form-floating" on:submit|preventDefault={handleSubmit}>
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
</form>
<form class="form-floating" on:submit|preventDefault={handleSubmit}>
    <div class="form-floating mb-3">
        <input
            bind:value={data.BillingPostalCode}
            type="text"
            class="form-control"
            class:is-invalid={billingPostalCodeIsValid}
            id="BillingPostalCodeInputField"
        />
        <label for="BillingPostalCodeInputField">Billing PostalCode</label>
    </div>
</form>
<form class="form-floating" on:submit|preventDefault={handleSubmit}>
    <div class="form-floating mb-3">
        <input
            bind:value={data.Total}
            type="text"
            class="form-control"
            class:is-invalid={totalIsValid}
            id="TotalInputField"
        />
        <label for="TotalInputField">Total</label>
    </div>
    <button type="submit" class="btn btn-primary">Submit</button>
    <a class="btn btn-dark" href={`/admin/artists`}>Back</a>
</form>
