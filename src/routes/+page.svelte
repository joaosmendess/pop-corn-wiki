<script lang="ts">
	import { onMount } from 'svelte';

	type MediaItem = {
		id: number;
		title: string;
		poster_path: string;
		media_type: 'movie' | 'tv';
		vote_average: number;
	};
	let mediaItems: MediaItem[] = [];
	let itemCount: number = 0;

	async function fetchMedia() {
		try {
			//movies

			const apiKey = 'b3fad328fadefd997a6ee64e219ac9cd';
			const baseUrl = 'https://api.themoviedb.org/3';
			const movieResponse = await fetch(`${baseUrl}/movie/popular?api_key=${apiKey}&language=pt-BR`);
			const movies = await movieResponse.json();

			//tvshow

			const tvResponse = await fetch(`${baseUrl}/tv/popular?api_key=${apiKey}&language=pt-BR`);
			const tvShows = await tvResponse.json();

			mediaItems = [
				...movies.results.map((item: any) => ({
					...item,
					title: item.title || item.name,
					media_type: 'movie',
					vote_average: item.vote_average
				})),
				...tvShows.results.map((item: any) => ({
					...item,
					title: item.title || item.name,
					media_type: 'tv',
					vote_average: item.vote_average

				}))
			];
			itemCount= mediaItems.length;
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
	<section class="aside">
		<h2>Em alta no momento 🔥 ({itemCount})  </h2>
	</section>
	<div class="grid">
		{#each mediaItems as item}
		<a href={`/details?id=${item.id}&type=${item.media_type}`}>

			<div class="card-wrapper">
				<div  class="card-image">
					<div class="card-rating">
						<span class="rating-icon">⭐ </span>
						{item.vote_average.toFixed(1)}
					</div>
					<img loading="lazy" src={`https://image.tmdb.org/t/p/w500${item.poster_path}`} alt={item.title} />
					<h2>{item.title} ({item.media_type.toUpperCase()})</h2>
					
				</div>
			</div>
		</a>
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
a{
	text-decoration: none;
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
.card-image  {
	position: relative;
    width: 100%;
    border-radius: .71rem;
    height: auto;
    object-fit: cover;
    transition: transform 0.3s ease; 
	cursor: pointer;
}
img {
	width: 100%;
    border-radius: .71rem;
    height: auto;
    object-fit: cover;
    transition: transform 0.3s ease; 
}
.card-image img:hover {
    transform: scale(1.05); 
}

.aside h2{
	display: flex;
	justify-content: baseline;
	margin-left: 60px;
	font-weight: 700;
	font-size: 1rem;
}
.card-rating {
	position: absolute;
    width: 20%;
    top: 1%;
    left: 3%;
    background-color: rgba(0, 0, 0, 0.5); 
    color: #ffd700; /* Gold color for the rating */
    padding: 5px 10px;
    border-radius: 5px;
    display: flex;
    align-items: center;
    font-size: 0.9rem;
    font-weight: bold;
	overflow: hidden;
	z-index: 1;
}

.rating-icon {
    margin-right: 5px;
}

@media (max-width: 768px) { 
    .grid {
        grid-template-columns: 1fr; 
        padding: 10px;
    }
    .card-wrapper {
        width: 80%; 
        margin: 0 auto; 
    }
    h2 {
        font-size: 1rem; 
    }
}
</style>
