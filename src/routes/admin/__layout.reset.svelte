<script>
    import { page } from "$app/stores";

    import Sidebar from "../../lib/components/Sidebar.svelte";

    const navItems = [
        {
            label: "Home",
            href: "/admin",
        },
        {
            label: "Artists",
            href: "/admin/artists",
        },
        {
            label: "Albums",
            href: "/admin/albums",
        },
        {
            label: "Customers",
            href: "/admin/customers",
        },
        {
            label: "Employees",
            href: "/admin/employees",
        },
        {
            label: "Genres",
            href: "/admin/genres",
        },

        {
            label: "Invoices",
            href: "/admin/invoices",
        },
        {
            label: "Media-Types",
            href: "/admin/media-types",
        },
        {
            label: "Playlists",
            href: "/admin/playlists",
        },
        {
            label: "Tracks",
            href: "/admin/tracks",
        },
    ];
</script>

<header>
    <nav class="navbar navbar-expand-md navbar-dark fixed-top bg-dark g-col-6">
        <div class="container-fluid grid">
            <a class="navbar-brand" href="/">Carousel</a>
            <button
                class="navbar-toggler"
                type="button"
                data-bs-toggle="collapse"
                data-bs-target="#navbarCollapse"
                aria-controls="navbarCollapse"
                aria-expanded="false"
                aria-label="Toggle navigation"
            >
                <span class="navbar-toggler-icon" />
            </button>
            <div class="collapse navbar-collapse" id="navbarCollapse">
                <ul class="navbar-nav me-auto mb-2 mb-md-0">
                    {#each navItems as { label, href }}
                        <li class="nav-item">
                            <a
                                class="nav-link"
                                class:active={$page.path === href}
                                aria-current="page"
                                {href}>{label}</a
                            >
                        </li>
                    {/each}
                </ul>
            </div>
        </div>
    </nav>
    <!-- <Sidebar /> -->
</header>

<main class="container">
    <slot />
</main>

<style>
    main {
        padding: 100px 0px;
    }
</style>
