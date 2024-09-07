<template>
  <NuxtLink :to="`/movies/${movie.id}`">
    <div class="flipCard">
      <div class="flipCard-inner">
        <div class="flipCard-front">
          <div class="content_img">
            <span class="rating">{{ roundedValue }}</span>
            <img :src="movie.poster_path" class="image" />
          </div>
        </div>

        <div class="flipCard-back">
          <div class="backCard">
            <p class="details">Info:</p>
            <div class="overview">
              {{ movie.overview }}
            </div>
          </div>
        </div>
      </div>
      <div class="baseOfCard">
        <p class="title">{{ movie.original_title }}</p>
        <p class="releaseDate">Released: {{ movie.release_date }}</p>
      </div>
    </div>
  </NuxtLink>
</template>

<script setup>
let { movie } = defineProps(['movie']);
const roundedValue = parseFloat(movie.vote_average.toFixed(1));
</script>

<style scoped>

.flipCard {
  height: 35rem;
  width: 20rem;
  display: flex;
  flex-direction: column;
  align-items: center;
  text-align: center;
  background-color: #1c1919;
  border-radius: 8px;
  perspective: 1000px;
  overflow: hidden;
}

.flipCard-inner {
  position: relative;
  width: 100%;
  height: 100%;
  transition: transform 0.6s;
  transform-style: preserve-3d;
}

.flipCard:hover .flipCard-inner {
  transform: rotateY(180deg);
}

.flipCard-front,
.flipCard-back {
  position: absolute;
  width: 100%;
  height: 100%;
  backface-visibility: hidden;
}

.flipCard-front {
  display: flex;
  flex-direction: column;
  justify-content: space-between; /* Adjusts the content to fill the space */
}

.flipCard-back {
  transform: rotateY(180deg);
}

.content_img {
  position: relative;
  width: 100%;
  flex-grow: 1; /* Allows image container to grow */
  display: flex;
  align-items: center;
  justify-content: center;
}

.image {
  width: 100%;
  height: 100%;
  object-fit: cover;
}

.rating {
  position: absolute;
  top: 10px;
  left: 10px;
  background-color: #ff4500;
  padding: 3px 6px;
  border-radius: 8px;
  color: white;
  font-weight: bold;
}

.baseOfCard {
  background-color: #1c1919;
  padding: 1rem;
  width: 100%;
  text-align: center;
  border-top: 1px solid #333;
  box-sizing: border-box; /* Ensures padding is included in height calculation */
}

.title {
  font-size: 1.2em;
  font-weight: bold;
  color: aqua;
  margin: 0;
}

.releaseDate {
  font-size: 1rem;
  color: #ff4500;
  margin: 0;
}

.details {
  color: #ff4500;
  font-family: 'Sigmar', cursive;
  font-size: 2em;
  text-align: center;
  margin: 0;
}

.overview {
  margin-top: 1rem;
  height: calc(100% - 3rem); /* Adjusts height to account for other content */
  color: white;
  overflow: auto;
  text-align: justify;
  padding: 1rem;
  text-overflow: ellipsis;
}

.overview::-webkit-scrollbar {
  display: none;
}

</style>
