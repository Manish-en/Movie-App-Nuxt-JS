<template>
    
    <div class="all">
        <div class="searchBar">
            <div style="width: 92%">
                <div class="innerSearchBar">
                    <input 
                    type="text" 
                    v-model="keyword"
                    class="searchInput" 
                    placeholder="Enter movie name" 
                    @keyup=searchController()
                    />
                    <button class="myBtn" @click="searchController()"> Search</button>
                </div>
            </div>
            <div style="padding-top:1.5vw" >
                <i 
                    class="material-icons navbtn" 
                    style="padding:5px 0px" 
                    @click="decrement" 
                    :class="page == 1 ? 'disable' : ''"
                >keyboard_double_arrow_left</i>
                <span class="pg" style="font-size: 1.7em;"> {{ page }} </span>
                <i 
                    class="material-icons navbtn" 
                    style="padding:5px 0px" 
                    @click="increment"
                    :class="totalItems < 20 ? 'disable' : ''"
                >keyboard_double_arrow_right</i>
            </div>
        </div>

        <div class="movieContainer">
            <div v-for="movie in movies" :key="movies.id" class="moviecard">
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
    background-color: black;
}
.searchBar {
    display: flex;
    position: sticky;
    top: 0;
    padding-bottom: 2vw;
    background: linear-gradient(180deg, #000, #232323);
    z-index: 1000;
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

.myBtn {
    font-size: 1.2rem;
    background-color: #ff4500;
    color: white;
    cursor: grab;
    border-top-right-radius: 8px;
    border-bottom-right-radius: 8px;
    padding: 6px 12px 7px 18px;
}
.myBtn:hover{
    transform: scale(1.1);
}

.innerSearchBar{
    margin-left: 6vw;
    margin-top: 2vw;
}
.searchInput {
    border-top-left-radius: 8px;
    border-bottom-left-radius: 8px;
    padding: 9px 15px;
}
.navbtn{
    color: aliceblue;
}
.navbtn:hover{
    color: aqua;
    cursor: pointer;
    transform: scale(1.5);
}
.pg{
    color: white;
    font-size: 2em;
    text-align: center;
    padding: 15px 9px 5px 9px;
}
.pg2{
    color: #ff4500;
    font-family: 'Sigmar', cursive;
    font-size: 2em;
    text-align:left;
    border: 2px solid #ff4500;
    width: fit-content;
    padding: 10px;
    margin-top: 1em;
    margin-left: 5em;

}

.disable{
    pointer-events: none;
    display: none;
    opacity: 0.3;
}

</style>
