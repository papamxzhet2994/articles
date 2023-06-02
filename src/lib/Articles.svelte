<script>
    import { onMount } from 'svelte';

    let articles = [];

    onMount(async () => {
        try {
            const apiKey = '6ef6673d11d5407e9c133a85d9cc6fd6';
            const response = await fetch(`https://newsapi.org/v2/everything?q=programming&from=2023-06-01&to=2022-06-01&sortBy=popularity&language=ru&apiKey=${apiKey}    `);
            if (response.ok) {
                const data = await response.json();
                articles = data.articles;
            } else {
                console.error('Ошибка при получении статей:', response.statusText);
            }
        } catch (error) {
            console.error('Ошибка при получении статей:', error);
        }
    });
</script>

<div class="container mx-auto">
    <h1 class="text-3xl font-bold mb-4">Статьи</h1>
    {#if articles.length > 0}
        <div class="cards-container">
            <div class="cards">
                {#each articles as article}
                    <div class="card">
                        <div class="border rounded-lg overflow-hidden shadow-lg w-64">
                            <div class="p-6">
                                {#if article.urlToImage}
                                    <img class="w-full object-cover h-48 rounded-xl" src="{article.urlToImage}" alt="{article.title}">
                                {:else}
                                    <div class="placeholder"></div>
                                {/if}
                                <h2 class="text-2xl font-bold mb-2">{article.title}</h2>
                                <p class="text-gray-700">{article.description}</p>
                                <a class="text-blue-500 hover:text-blue-700" href="{article.url}" target="_blank" rel="noopener">Читать далее</a>
                            </div>
                        </div>
                    </div>
                {/each}
            </div>
        </div>
    {:else}
        <p class="text-gray-700">Загрузка статей...</p>
    {/if}
</div>


<style>
    .container {
        padding: 1rem;
        max-width: 500px;
        margin: 0 auto;
    }

    .cards {
        display: grid;
        grid-template-columns: repeat(3, 1fr);
        grid-gap: 0px;
    }

    .cards-container {
        display: grid;
        grid-template-columns: repeat(auto-fill, minmax(300px, 1fr));
        grid-gap: 1rem;
        justify-items: center;
    }


    .card {
        margin: 0.5rem;
    }

    .border {
        border-width: 1px;
        border-color: rgba(209, 213, 219, var(--tw-border-opacity));
    }

    .shadow-lg {
        --tw-shadow: 0 10px 15px -3px rgba(0, 0, 0, 0.1), 0 4px 6px -2px rgba(0, 0, 0, 0.05);
        box-shadow: var(--tw-ring-offset-shadow, 0 0 #0000), var(--tw-ring-shadow, 0 0 #0000), var(--tw-shadow);
    }

    .p-6 {
        padding: 1.5rem;
        border-radius: 15px;
        background-color: #fff;
        box-shadow: 0 10px 15px -3px rgba(0, 0, 0, 0.1), 0 4px 6px -2px rgba(0, 0, 0, 0.05);
        color: #1a1a1a;
        margin-bottom: 0;
    }

    img {
        border-radius: 10px;
        width: 100%;
        height: 100%;
        object-fit: cover;
    }

    .placeholder {
        width: 100%;
        height: 200px;
        background-color: #eee;
        border-radius: 10px;
    }
</style>

