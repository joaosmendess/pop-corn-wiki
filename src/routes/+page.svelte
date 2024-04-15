<script lang="ts">
	import { onMount } from 'svelte';

	type MediaItem = {
		id: number;
		title: string;
		poster_path: string;
		media_type: 'movie' | 'tv';
	};
	let mediaItems: MediaItem[] = [];

	async function fetchMedia() {
		try {
			//movies

			const apiKey = 'b3fad328fadefd997a6ee64e219ac9cd';
			const baseUrl = 'https://api.themoviedb.org/3';
			const movieResponse = await fetch(`${baseUrl}/movie/popular?api_key=${apiKey}`);
			const movies = await movieResponse.json();

			//tvshow

			const tvResponse = await fetch(`${baseUrl}/tv/popular?api_key=${apiKey}`);
			const tvShows = await tvResponse.json();

			mediaItems = [
				...movies.results.slice(0, 6).map((item: any) => ({
					...item,
					title: item.title || item.name,
					media_type: 'movie'
				})),
				...tvShows.results.slice(0, 6).map((item: any) => ({
					...item,
					title: item.title || item.name,
					media_type: 'tv'
				}))
			];
		} catch (error) {
			console.error('Error fetching media', error);
		}
	}
	onMount(fetchMedia);
</script>

<svelte:head>
	<title>Home</title>
	<meta name="description" content="Svelte demo app" />
</svelte:head>

<main>
	<h1>Popular Media</h1>
	<div class="grid">
		{#each mediaItems as item}
			<div class="card-wrapper">
				<div  class="card-image">
					<img loading="lazy" src={`https://image.tmdb.org/t/p/w500${item.poster_path}`} alt={item.title} />
					<h2>{item.title} ({item.media_type.toUpperCase()})</h2>
				</div>
			</div>
		{/each}
	</div>
</main>

<style>
:global(body) {
    margin: 0;
    display: flex;
    justify-content: center;
    align-items: center;
    padding: 0;
    background: #0b0c12;
    color: #fff; /* Define a cor do texto para branco */
    font-family: 'Poppins', sans-serif;
    height: 100%; /* Garante que o fundo cubra toda a altura */
}
.grid {
    display: grid;
    grid-template-columns: repeat(4, 1fr); /* 4 colunas em desktop */
    justify-content: center;
    gap: 20px;
    padding: 20px;
    align-items: start; /* Alinhamento no começo para evitar distorções */
}
.card-wrapper {
    display: flex;
    align-content: center;
    justify-content: center; 
    align-items: center; 
    background-color: #1a1a1a;
    border-radius: .71rem;
    width: 75%; /* Ajuste para melhor visualização em desktop */
    padding: .5rem;
    height: auto; /* Altura automática para se adaptar ao conteúdo */
	margin-left: 30px;
}

h1 {
    text-align: center;
}
h2 {
    font-weight: 500;
    font-size: .8rem;
    text-decoration: none;
    color: #fff;
    margin-top: 15px;
}
.card-image img {
    width: 100%;
    border-radius: .71rem;
    height: auto;
    object-fit: cover;
    transition: transform 0.3s ease; /* Adiciona uma transição suave */
	cursor: pointer;
}

.card-image img:hover {
    transform: scale(1.05); /* Move a imagem 10px para cima */
}

@media (max-width: 768px) { /* Ajustes para telas menores que 768px */
    .grid {
        grid-template-columns: 1fr; /* 1 coluna em telas pequenas */
        padding: 10px;
    }
    .card-wrapper {
        width: 80%; /* Ocupa a largura toda em telas pequenas */
        margin: 0 auto; /* Centraliza */
    }
    h2 {
        font-size: 1rem; /* Aumenta a fonte para leitura em dispositivos móveis */
    }
}
</style>
