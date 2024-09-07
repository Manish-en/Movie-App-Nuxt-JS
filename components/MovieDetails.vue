<template>
  <div class="page-container">
    <div class="header">
      <NuxtLink to="/">
        <button class="btn">Back</button>
      </NuxtLink>
    </div>

    <div class="movie-detail">
      <div class="info">
        <div class="imgblock">
          <img :src="image" alt="Movie Poster" />
        </div>
        <div class="description">
          <h1>{{ movie.original_title }}</h1>
          <p>Status: {{ movie.status }}</p>
          <p>Released: {{ movie.release_date }}</p>
          <p>Budget: {{ movie.budget }}</p>
          <p>Language: {{ movie.original_language }}</p>
          <p>Popularity: {{ movie.popularity }}</p>
          <p>Runtime: {{ movie.runtime }} min</p>
          <p class="overview">{{ movie.overview }}</p>
          <button class="download-btn" disabled>Download</button>
        </div>
      </div>
      <!-- Placeholder for the MovieReview Component -->
      <div class="review-placeholder">
        <MovieReview :id="movie.id" />
      </div>
    </div>
  </div>
</template>

<script setup>
import store from 'store2'

useHead({
  title: 'Movie Details'
})

const { movie } = defineProps(['movie'])

let image = `https://image.tmdb.org/t/p/w500//${movie.backdrop_path}`;
if (movie.backdrop_path == null) {
  image = 'https://st3.depositphotos.com/1322515/35964/v/600/depositphotos_359648638-stock-illustration-image-available-icon.jpg';
}

store.set('movieId', movie.id);
</script>

<style scoped>
/* Container for the entire page */
.page-container {
  background: linear-gradient(135deg, #2c3e50, #4ca1af); /* Dark, cinematic gradient */
  min-height: 100vh;
  display: flex;
  flex-direction: column;
  align-items: center; /* Center content horizontally */
  padding: 1rem;
  box-sizing: border-box;
}

/* Back button styling */
.header {
  width: 100%;
  display: flex;
  justify-content: flex-start;
  margin-bottom: 1rem;
}

.btn {
  background-color: #e74c3c; /* Vibrant color for visibility */
  color: white;
  border: 1px solid #e74c3c;
  padding: 8px 12px;
  cursor: pointer;
  border-radius: 8px;
  transition: background-color 0.3s, border-color 0.3s;
}

.btn:hover {
  background-color: #c0392b; /* Darker shade on hover */
  border-color: #c0392b;
}

/* Main container for the movie details and review section */
.movie-detail {
  display: flex;
  width: 100%;
  max-width: 1200px;
  justify-content: space-between;
  flex-grow: 1;
  gap: 1rem;
  align-items: flex-start;
  flex-wrap: wrap; /* Wrap items for responsiveness */
}

/* About section which includes movie info and image */
.info {
  display: flex;
  flex-direction: column;
  align-items: center; /* Center image and details vertically */
  width: 60%;
  background: rgba(0, 0, 0, 0.7); /* Semi-transparent background */
  border-radius: 12px;
  box-shadow: 0 4px 20px rgba(0, 0, 0, 0.5); /* Subtle shadow for depth */
  padding: 1rem;
  box-sizing: border-box;
}

/* Flexbox for image and description */
.imgblock {
  width: 100%;
  margin-bottom: 1rem;
  border-radius: 12px;
  overflow: hidden;
}

img {
  width: 100%;
  height: auto;
  border-radius: 12px;
  transition: transform 0.3s;
}

img:hover {
  transform: scale(1.05); /* Slight zoom on hover */
}

/* Movie description styling */
.description {
  width: 100%;
  background-color: #1c1c1c; /* Dark background for text contrast */
  padding: 1.5rem;
  border-radius: 12px;
  color: white;
  text-align: left;
}

h1 {
  font-weight: bold;
  font-size: 2.5rem;
  color: #ecf0f1; /* Light color for the movie title */
  margin-bottom: 0.5rem;
  text-align: center; /* Center the title */
}

p {
  color: #bdc3c7; /* Softer color for other text */
  margin: 0.5rem 0;
}

/* Overview paragraph styling */
.overview {
  font-size: 1.2rem;
  color: #ecf0f1;
  margin-top: 1rem;
  line-height: 1.6;
  text-align: justify;
}

/* Download button styling */
.download-btn {
  background-color: #555;
  color: white;
  border: none;
  padding: 10px 15px;
  border-radius: 8px;
  cursor: not-allowed;
  margin-top: 1rem;
  font-size: 1rem;
  transition: background-color 0.3s;
}

.download-btn:disabled {
  background-color: #333;
}

/* Placeholder for the MovieReview component */
.review-placeholder {
  width: 35%; /* Reserve 35% width for the movie review section */
  background: rgba(0, 0, 0, 0.7); /* Semi-transparent background */
  border-radius: 12px;
  box-shadow: 0 4px 20px rgba(0, 0, 0, 0.5); /* Subtle shadow for depth */
  padding: 1rem;
  box-sizing: border-box;
}

/* Responsive design adjustments */
@media (max-width: 1024px) {
  .movie-detail {
    flex-direction: column;
    align-items: center;
  }

  .info,
  .review-placeholder {
    width: 100%;
  }

  .review-placeholder {
    margin-top: 1rem;
  }
}

@media (max-width: 768px) {
  h1 {
    font-size: 2rem;
  }

  .overview {
    font-size: 1rem;
  }

  .download-btn {
    padding: 8px 12px;
    font-size: 0.9rem;
  }
}
</style>
