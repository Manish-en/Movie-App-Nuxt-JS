<template>
    <nav>
        <NuxtLink to="/"></NuxtLink>
    </nav>
    <div>
        <header 
            class="heading" 
            :style="{'backgroundImage':'url('+image+')'}"
        >
            <div class="overlay">
                <div class="headclass">
                    <p>
                        <i class="material-symbols-outlined">movie</i>
                        Mystic Movie
                    </p>
                    <h1 v-if="id != null">
                        <NuxtLink :to="`/movies/${id}`" class="link" >
                            Now Streaming: {{ movieTitle }}
                        </NuxtLink>
                    </h1>
                </div>
            </div>
        </header>
    </div>
    <!-- output page content -->
    <div class="content">
        <slot/>
    </div>
</template>

<script setup>
import { ref } from 'vue'
import store from 'store2'

const result = ref({});
const image = ref('');
const movieTitle = ref('');

const id = ref(store.get('movieId'));

if(id.value !== null){
    async function getSingleMovie(){
        const response = await fetch(`https://api.themoviedb.org/3/movie/${id.value}?api_key=bb5c9a25161603cb7d1205e55e4cbe88&language=en-US`)
        const data = await response.json();
        
        result.value = data;
        result.value['poster_path'] = 'https://image.tmdb.org/t/p/w500/'+ result.value.poster_path;
        result.value['backdrop_path'] = 'https://image.tmdb.org/t/p/w500/'+ result.value.backdrop_path;

        image.value = result.value.backdrop_path;
        movieTitle.value = result.value.original_title;
    }
    getSingleMovie();

    store.remove('movieId');
}else{
    image.value = 'https://images.unsplash.com/photo-1574267432306-5ddbe53bda16?ixlib=rb-4.0.3&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=1631&q=80'
}
</script>

<style scoped>
nav {
    padding: 1rem;
}

.heading {
    position: relative;
    width: 100%;
    height: 30vh; /* Adjust height as needed */
    background-position: center;
    background-repeat: no-repeat;
    background-size: cover;
    color: white;
    display: flex;
    align-items: center;
    justify-content: center;
}

.overlay {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background: rgba(0, 0, 0, 0.5); /* Optional: dark overlay for better text visibility */
    display: flex;
    align-items: center;
    justify-content: center;
}

.headclass {
    text-align: center;
}

p {
    font-family: 'Sigmar', cursive;
    font-size: 2.5rem;
    margin: 0;
    padding: 0.5rem 1rem;
    border-radius: 25px;
}

p:hover {
    color: #ff4500;
}

h1 {
    font-weight: bolder;
    font-size: 2rem;
    margin: 1rem 0;
}

.material-symbols-outlined {
    font-size: 2.4rem;
}

.link {
    color: white;
    text-decoration: none;
}

.link:hover {
    text-decoration: underline;
    color: #ff4500;
}

.content {
    padding: 2rem;
}

/* Responsive adjustments
@media (max-width: 768px) {
    .heading {
        height: 40vh;
    }
    .headclass p {
        font-size: 2rem;
    }
    .headclass h1 {
        font-size: 1.5rem;
    }
}

@media (max-width: 480px) {
    .heading {
        height: 50vh;
    }
    .headclass p {
        font-size: 1.5rem;
    }
    .headclass h1 {
        font-size: 1.2rem;
    }
} */
</style>
