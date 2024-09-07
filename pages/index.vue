<template>
    <div class="all">
        <div class="searchBar">
            <div class="innerSearchBar">
                <input 
                    type="text" 
                    v-model="keyword"
                    class="searchInput" 
                    placeholder="Enter movie name" 
                    @keyup="searchController()"
                />
                <button class="myBtn" @click="searchController()">Search</button>
            </div>
            <div class="pagination">
                <i 
                    class="material-icons navbtn" 
                    @click="decrement" 
                    :class="page == 1 ? 'disable' : ''"
                >keyboard_double_arrow_left</i>
                <span class="pg">{{ page }}</span>
                <i 
                    class="material-icons navbtn" 
                    @click="increment"
                    :class="totalItems < 20 ? 'disable' : ''"
                >keyboard_double_arrow_right</i>
            </div>
        </div>
        <div class="movieContainer">
            <div v-for="movie in movies" :key="movie.id" class="movieCard">
                <MovieCard :movie="movie"/>
            </div>
        </div>
    </div>
</template>
<script setup>
import store from "store2"

definePageMeta({
    layout: 'home'
})

let keyword = ref('');
let receivedKeyword = ref(null);


let result = ref({});
let popular = ref({});

let movies = ref([]);
let popularMovies = ref([]);
let index = ref(0);

let hit = false;
let totalItems = ref();
let page = ref(1);

receivedKeyword.value = store.get('receivedKeyword');


function increment(){
    page.value++;
    if(hit == true){
        searchMovies()
    }else{
        getNowMovies()
    }
}
function decrement(){
    if(page.value > 1){
        
        page.value--;
        if(hit == true){
            searchMovies()
        }else{
            getNowMovies()
        }
    }
}

function searchController(){
    
    if(keyword.value == ''){

        hit = false;
        page.value = 1;
        store.remove('receivedKeyword');
        getNowMovies()
    }else{
        
        changeToOne();
        searchMovies();
    }
}

function changeToOne(){
    if(keyword.value !== ''){
        page.value = 1;
    }
}

async function getNowMovies(){


    popular = await fetch(`https://api.themoviedb.org/3/movie/popular?api_key=bb5c9a25161603cb7d1205e55e4cbe88&language=en-US&page=${page.value}`)
        .then((response) => {
            return response.json();
        })

    
    
        popular.results.map((x) => {
            if(x.poster_path == null){
                x['poster_path'] = 'https://st3.depositphotos.com/1322515/35964/v/600/depositphotos_359648638-stock-illustration-image-available-icon.jpg';
            }else{
                x['poster_path'] = 'https://image.tmdb.org/t/p/w500/'+ x.poster_path;
            }
    })


    popularMovies.value = popular.results;
    
    console.log(" Get Now Movies Called ");
    result = await fetch(`https://api.themoviedb.org/3/movie/now_playing?api_key=bb5c9a25161603cb7d1205e55e4cbe88&page=${page.value}`)
        .then((response) => {
            return response.json();
        })

    result.results.map((x) => {
            if(x.poster_path == null){
                x['poster_path'] = 'https://st3.depositphotos.com/1322515/35964/v/600/depositphotos_359648638-stock-illustration-image-available-icon.jpg';
            }else{
                x['poster_path'] = 'https://image.tmdb.org/t/p/w500/'+ x.poster_path;
            }
    });

    movies.value = result.results;

}

async function searchMovies(){
    console.log('Search Movies Called');
    hit == false ? hit = true : null;

    if(keyword.value !== ''){
        store.set('receivedKeyword', keyword.value)

        result = await fetch(`https://api.themoviedb.org/3/search/movie?page=1&api_key=bb5c9a25161603cb7d1205e55e4cbe88&query=${keyword.value}&page=${page.value}`)
        .then((response) => response.json())

    result.results.map((x) => {
        if(x.poster_path == null){
                x['poster_path'] = 'https://st3.depositphotos.com/1322515/35964/v/600/depositphotos_359648638-stock-illustration-image-available-icon.jpg';
            }else{
                x['poster_path'] = 'https://image.tmdb.org/t/p/w500/'+x.poster_path;
            }
    });

    movies.value =  result.results
    totalItems.value = result.results.length;
    }
}

if(receivedKeyword.value !== null){
    keyword.value = receivedKeyword.value;
    await searchMovies();
}else {
    await getNowMovies()
}

</script>



<style scoped>

.all {
    background-color: linear-gradient(180deg, #000, #232323);
}
.searchBar {
    display: flex;
    flex-direction: column;
    align-items: center;
    position: sticky;
    top: 0;
    padding: 1.5rem 0;
    background: linear-gradient(180deg, #000, #232323);
    z-index: 1000;
}

.innerSearchBar {
    display: flex;
    align-items: center;
    width: 90%;
    max-width: 800px;
    margin-bottom: 1rem;
}

.searchInput {
    flex: 1;
    border: none;
    border-radius: 8px 0 0 8px;
    padding: 10px;
    font-size: 1rem;
    outline: none;
}

.myBtn {
    background-color: #ff4500;
    color: white;
    border: none;
    border-radius: 0 8px 8px 0;
    padding: 10px 20px;
    font-size: 1rem;
    cursor: pointer;
}

.myBtn:hover {
    background-color: #e03d00;
    transform: scale(1.05);
}

.pagination {
    display: flex;
    align-items: center;
    gap: 1rem;
}

.navbtn {
    color: aliceblue;
    font-size: 1.5rem;
    cursor: pointer;
    transition: color 0.3s, transform 0.3s;
}

.navbtn:hover {
    color: aqua;
    transform: scale(1.2);
}

.pg {
    color: white;
    font-size: 1.5rem;
}

.disable {
    pointer-events: none;
    opacity: 0.3;
}


.movieContainer{
    display: flex;
    flex-wrap: wrap;
    justify-content: center;
    gap: 20px;
    padding: 20px;
}
.moviecard {
    margin-left: 3vw;
    margin-top: 4vw;
    text-align: center;
    display: inline-flex;
}
.moviecard:hover{
    transform: scale(1.1);
}



</style>
