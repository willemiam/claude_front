<script>
    import { filterItems, fetchMakes, fetchModels } from "../lib/api.js";
    let make = "";
    let model = "";
    let year = "";
    let items = [];
    let makes = [];
    let models = [];

    // Pobierz listę marek przy załadowaniu komponentu
    async function loadMakes() {
        makes = await fetchMakes();
    }
    loadMakes();

    // Kiedy użytkownik wybierze markę, pobierz dostępne modele
    async function updateModels() {
        if (make) {
            models = await fetchModels(make);
        } else {
            models = [];
        }
        model = ""; // Resetujemy model po zmianie marki
    }

    async function applyFilter() {
        items = await filterItems(make, model, year);
    }
</script>

\<!-- <section class="hero">
    <div class="container">
        <h1>Find Your Perfect Car</h1>
        <p>
            Browse thousands of new and used cars from trusted dealers and
            private sellers
        </p>

        <div class="search-box">
            <form class="search-form" on:submit={handleSearch}>
                <select bind:value={make}>
                    <option value="">All Makes</option>

                </select>

                <select bind:value={model}>
                    <option value="">All Models</option>
                </select>

                <select bind:value={year}>
                    <option value="">Any Price</option>
                </select>

                <a href="/filter" class="nav-link">Filtruj Ogłoszenia</a>
            </form>
        </div>
    </div>
</section> -->
<section class="main-baner">
    <div class="container">
        <h1>Find Your Perfect Car</h1>
        <p>
            Browse thousands of new and used cars from trusted dealers and
            private sellers
        </p>
        <div class="search-box">
            <form class="search-form">
                <select bind:value={make} on:change={updateModels}>
                    <option value="">Wybierz markę</option>
                    {#each makes as brand}
                        <option value={brand}>{brand}</option>
                    {/each}
                </select>

                <!-- Lista rozwijana dla modeli -->
                <select bind:value={model} disabled={!make}>
                    <option value="">Wybierz model</option>
                    {#each models as m}
                        <option value={m}>{m}</option>
                    {/each}
                </select>

                <input type="number" bind:value={year} placeholder="Rok" />
                <button on:click={applyFilter}>Szukaj</button>
            </form>
        </div>
    </div>

    {#if items.length > 0}
        <ul>
            {#each items as item}
                <li>
                    <strong>{item.make} {item.model} ({item.year})</strong> - {item.price}
                    PLN
                </li>
            {/each}
        </ul>
    {:else}
        <p>Brak wyników filtrowania.</p>
    {/if}
</section>

<style>
    .main-baner {
        background:
            linear-gradient(rgba(0, 0, 0, 0.5), rgba(0, 0, 0, 0.5)),
            url("/images/hero-car.jpg") center/cover no-repeat;
        color: white;
        padding: 4rem 0;
        text-align: center;
    }

    .main-baner h1 {
        font-size: 2.5rem;
        margin-bottom: 1rem;
    }

    .main-baner p {
        font-size: 1.2rem;
        margin-bottom: 2rem;
        max-width: 700px;
        margin-left: auto;
        margin-right: auto;
    }

    .search-box {
        background-color: white;
        padding: 2rem;
        border-radius: 8px;
        max-width: 800px;
        margin: 0 auto;
        box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
    }

    .search-form {
        display: grid;
        grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
        gap: 1rem;
    }

    .search-form select {
        padding: 0.75rem;
        border: 1px solid var(--mid-gray);
        border-radius: 4px;
        font-size: 1rem;
    }

    .search-form button {
        padding: 0.75rem;
        font-size: 1rem;
    }
</style>
