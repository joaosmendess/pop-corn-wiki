<script lang="ts">
  import { onMount } from 'svelte';

  interface Genre {
    name: string;
  }

  interface MediaDetail {
    title?: string;
    poster_path?: string;
    overview?: string;
    release_date?: string;
    genres?: Genre[];
    runtime?: number;
  }

  let mediaDetail: MediaDetail = {};

  onMount(async () => {
    const urlParams = new URLSearchParams(window.location.search);
    const id = urlParams.get('id');
    const mediaType = urlParams.get('type') || 'movie'; // Assegura um valor padrão

    const apiKey = 'b3fad328fadefd997a6ee64e219ac9cd';
    const baseUrl = 'https://api.themoviedb.org/3';

    if (id && mediaType) {
      const response = await fetch(`${baseUrl}/${mediaType}/${id}?api_key=${apiKey}`);
      if (response.ok) {
        mediaDetail = await response.json();
      } else {
        console.error('Failed to fetch details:', response.status);
      }
    } else {
      console.error('Missing parameters: id or type');
    }
  });
</script>


<svelte:head>
  <title>Details</title>
  <meta name="description" content="Svelte demo app" />
</svelte:head>

<main>
  {#if mediaDetail && mediaDetail.poster_path}
    <img src={`https://image.tmdb.org/t/p/w500${mediaDetail.poster_path}`} alt={mediaDetail.title || 'No title'}>
    <h1>{mediaDetail.title || 'No Title'}</h1>
    <p>{mediaDetail.overview || 'No description available'}</p>
    <p>Release date: {mediaDetail.release_date || 'Unknown'}</p>
    <p>Genres: {mediaDetail.genres && mediaDetail.genres.length ? mediaDetail.genres.map(genre => genre.name).join(', ') : 'No genres listed'}</p>
    <p>Duration: {mediaDetail.runtime ? `${mediaDetail.runtime} minutes` : 'Duration unknown'}</p>
  {/if}
</main>
