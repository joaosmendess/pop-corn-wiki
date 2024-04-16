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
    vote_average: number;
    backdrop_path?: string;
  }

  let mediaDetail: MediaDetail = {
    vote_average: 0
  };

  onMount(async () => {
    const urlParams = new URLSearchParams(window.location.search);
    const id = urlParams.get('id');
    const mediaType = urlParams.get('type') || 'movie';

    const apiKey = 'b3fad328fadefd997a6ee64e219ac9cd';
    const baseUrl = 'https://api.themoviedb.org/3';

    if (id && mediaType) {
      const response = await fetch(`${baseUrl}/${mediaType}/${id}?api_key=${apiKey}&language=pt-BR`);
      if (response.ok) {
        mediaDetail = await response.json();
      } else {
        console.error('Failed to fetch details:', response.status);
      }
    } else {
      console.error('Missing parameters: id or type');
    }
  });

  function goBack() {
    window.history.back();
  }
</script>

<svelte:head>
  <title>Details</title>
  <meta name="description" content="Svelte demo app" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
</svelte:head>

<main>
  <button on:click={goBack} class="back-button">
    &#8592; 
  </button>
  {#if mediaDetail && mediaDetail.poster_path}
  <div class="banner-image">
    <img class="img-header" src={`https://image.tmdb.org/t/p/w500${mediaDetail.poster_path}`} alt={mediaDetail.title || 'titulo nao encontrado'}>
    <div class="card-details">

      <h1>{mediaDetail.title || 'No Title'}</h1>
    </div>
  </div>

  <div class="grid">
    <section>
      <img class="img-details" src={`https://image.tmdb.org/t/p/original${mediaDetail.backdrop_path || mediaDetail.poster_path}`} alt={mediaDetail.title || 'No title'}>
    </section>
    <section class="info">
      <p class="description"> {mediaDetail.overview || 'No description available'}</p>
      <p>
        <span>Data de lançamento:</span> {mediaDetail.release_date || 'Unknown'}</p>
      <div class="card-rating">Nota:<span class="star"> {mediaDetail.vote_average.toFixed(1)} </span></div>
      <p><span>Gênero:</span> {mediaDetail.genres && mediaDetail.genres.length ? mediaDetail.genres.map(genre => genre.name).join(', ') : 'No genres listed'}</p>
      <p> <span>Duração:</span> {mediaDetail.runtime ? `${mediaDetail.runtime} minutes` : 'Duration unknown'}</p>
    </section>
  </div>
  {/if}
</main>

<style>
  :global(body) {
    margin: 0;
    display: flex;
    justify-content: center;
    align-items: center;
    padding: 0;
    background: #0b0c12;
    color: #fff;
    font-family: 'Poppins', sans-serif;
    height: 100%;
    overflow-x: hidden;
  }
  .banner-image {
    display: flex;
    justify-content: center;
    flex-direction: column;
    align-items: center;
    width: 65vw;
    border-radius: 2.5rem;
  text-align: center;
    margin: 35px auto;
  }
  .grid {
    display: grid;
    grid-template-columns: 600px 1fr;
    justify-content: center;
    padding: 35px;
    align-items: center;
  }
  .img-header {
    object-fit: cover;
    width: 100%;
    border-radius: 2.5rem;
    height: 500px;
  }
  .card-details h1 {
    font-weight: 300;
  display: flex;
  justify-content: baseline;
  align-items: center;
    
    border-radius: 24px;
    width: 100%;
    max-width: 375px;
    background: #1a1a1a;
   
    padding: 30px;
    text-align: center;
  }
  .card-rating {
    width: 20%;
    color: #767E94;
    border-radius: 5px;
    display: flex;
    align-items: center;
    font-size: 0.9rem;
    font-weight: bold;
    overflow: hidden;
    z-index: 1;
  }
  .star {
    margin-left: 7px;
    color: #ffd700;
    background-color: rgba(0, 0, 0, 0.9);
    border-radius: .5rem;
    width: auto;
    padding: 5px;
    text-align: center;
  }
  .img-details {
    height: 40rem;
    margin-top: 15px;
    border-radius: .71rem;
    width: 30rem;
  }
  .description {
    color: #767E94;
    text-align: left;
  }
  span {
    color: #767E94;
    text-align: left;
    margin-right: 5px;
    background-color: transparent;
  }
  .back-button {
  position: fixed;
  top: 20px;
  left: 20px;
  background: #1a1a1a;
  color: #fff;
  border: none;
  padding: 10px 20px;
  border-radius: 5px;
  cursor: pointer;
  font-size: 16px;
  z-index: 1000;
}

@media (max-width: 480px) {
  .back-button {
    font-size: 14px;
    padding: 8px 16px;
    top: 10px;
    left: 10px;
  }
}
  @media (max-width: 480px) {
    .grid {
      grid-template-columns: 350px;
      padding: 1px;
    }
    .card-details h1 {
      display: flex;
    font-weight: 300;
    
justify-content: baseline;
align-items: baseline;
    border-radius: 24px;
    width: 100%;
   
    background: #1a1a1a;
    
   
    padding: 10px;
    text-align: center;
  }
    .banner-image {
      display: flex;
      flex-direction: column;
      justify-content: center;
      align-items: center;
      height: auto; 
      width: 100vw;
      border-radius: 0rem;
      background: transparent;
      margin-top: 0;
      border: none;
    }
   
    .img-details {
      border: none;
      border-radius: 0rem;
      width: 100%;
    }
    .img-header {
      border: none;
      border-radius: 0rem;
      height: auto;
      width: 100%;
    }
    .info{
      padding: 10px;
      text-align: left;
    }
    .star {
    margin-left: 7px;
    color: #ffd700;
    background-color: rgba(0, 0, 0, 0.9);
    border-radius: .5rem;
    width: auto;
    
    text-align: center;
  }
  .card-rating {
    width: 28%;
    color: #767E94;
    border-radius: 5px;
    display: flex;
    align-items: center;
    font-size: 0.9rem;
    font-weight: bold;
    overflow: hidden;
    z-index: 1;
  }
  }
  @media (max-width: 400px) {
    .card-details  {
 text-align: center;
   
  }
  }
</style>
