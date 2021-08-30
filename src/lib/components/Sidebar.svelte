<script>
    import { page } from "$app/stores";
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

<div class="side-bar col">
    <div
        class="d-flex flex-column flex-shrink-0 p-3 text-white bg-dark"
        style="width: 280px;"
    >
        <a
            href="/"
            class="d-flex align-items-center mb-3 mb-md-0 me-md-auto text-white text-decoration-none"
        >
            <span class="fs-4">Sidebar</span>
        </a>
        <hr />
        <ul class="nav nav-pills flex-column mb-auto">
            {#each navItems as { label, href }}
                <li class="nav-item ">
                    <a
                        class="nav-link text-white"
                        class:active={$page.path === href}
                        aria-current="page"
                        {href}>{label}</a
                    >
                </li>
            {/each}
        </ul>
        <hr />
    </div>
</div>
