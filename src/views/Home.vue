<template>
  <div class="home">
    <div class="showcase">
      <div class="video-container">
        <video src="../assets/bg.mp4" autoplay v-bind:muted="true" loop></video>
      </div>
      <div class="content">
        <h1><span>Vue</span>Movies</h1>
      </div>
    </div>

    <form @submit.prevent="SearchMovies()" class="search-box">
      <input type="text" placeholder="What are you looking for?" v-model="search">
      <button type="submit"><i class="fa-solid fa-magnifying-glass"></i></button>
    </form>

    <h1 class="searchResult" v-if="searchResult">Results for "{{ searchResult }}"</h1>
    <div class="movies-list">
      <div class="movie" v-for="movie in movies" :key="movie.imdbID">
        <router-link :to="'/movie/' + movie.imdbID" class="movie-link">
          <div class="product-image">
            <img :src="movie.Poster" alt="Movie Poster">
            <div class="type">{{ movie.Type }}</div>
            <div class="detail">
              <h3>{{ movie.Title }}</h3>
              <p class="year">{{ movie.Year }}</p>
            </div>
          </div>
          <div class="overlay">
              <h3>{{ movie.Title }}</h3>
              <p>{{ movie.Year }}</p>
          </div>
        </router-link>
      </div>
    </div>
  </div>
</template>

<script>
import { ref } from 'vue';
import env from '@/env.js';

export default {
  setup(){
    const search = ref("");
    const movies = ref([]);
    const searchResult = ref("");
    const SearchMovies = () => {
      if (search.value != ""){
        fetch(`https://www.omdbapi.com/?apikey=${env.apikey}&s=${search.value}`)
        .then(response => {
          console.log(`https://www.omdbapi.com/?apikey=${env.apikey}&s=${search.value}`)
          return response.json()
        })
        .then(data => {
          console.log(data)
          let tmp = data.Search;
          let res = []
          let ids = []
          for (let i = 0; i < tmp.length; i++){
            let movie = tmp[i];
            if ((!ids.includes(movie.imdbID)) && (movie.Poster != "N/A")){
              ids.push(movie.imdbID)
              res.push(movie)
            }
          }
          movies.value = res;
          searchResult.value = search.value;
          search.value = "";
        })
      }
    }

    return {
      search,
      movies,
      SearchMovies,
      searchResult
    }
  }
}
</script>

<style lang="scss">

.home {

  .showcase{
    margin: 0;
    padding: 0;
    height: 50vh;
    display: flex;
    justify-content: center;
    text-align: center;
    color: white;

    .content{
      z-index: 1;
      display: flex;
      align-items: center;

      h1{
        font-size: 5em;

        @media only screen and (max-width: 500px) {
          font-size: 3.5em;
        }
      }
    }

  }

  .video-container{
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 50vh;
    overflow: hidden;
    background: rgba(0,0,0,0.8);
    z-index: 0;

    video{
      min-width: 100%;
      position: absolute;
      top: 50%;
      left: 50%;
      transform: translate(-50%, -50%);
      object-fit: cover;
      filter: brightness(50%);
    }
  }

  .feature-card {
    position: relative;

    .featured-img {
      display: block;
      width: 100%;
      height: 300px;
      object-fit: cover;
      position: relative;
      z-index: 0;
    }

    .detail {
      position: absolute;
      left: 0;
      right: 0;
      bottom: 0;
      background-color: rgba(0, 0, 0, 0.6);
      padding: 16px;
      z-index: 1;

      h3 {
        color: #FFF;
        margin-bottom: 16px;
      }

      p {
        color: #FFF;
      }
    }
  }

  .search-box {
    display: flex;
    flex-direction: row;
    justify-content: center;
    align-items: center;
    column-gap: 20px;
    padding: 16px;

    input {
      display: block;
      appearance: none;
      border: none;
      outline: none;
      background: none;

      &[type="text"] {
        width: 100%;
        max-width: 600px;
        font-size: 20px;
        padding: 10px 16px;
        border-radius: 30px;
        transition: 0.4s;
        border: 1px solid rgba(0,0,0,0.4);
        box-shadow: rgba(0, 0, 0, 0.16) 0px 1px 4px;

        &::placeholder {
          color: rgba(0,0,0,0.5);
        }

        &:focus {
          box-shadow: 0px 3px 6px rgba(0, 0, 0, 0.2);
            &::placeholder {
              opacity: 0;
          }
        }
      }
    }
    button{
      border: none;
      background-color: #42B883;
      padding: 10px 13px;
      border-radius: 30px;
      color: #FFF;
      font-size: 20px;
      text-transform: uppercase;
      transition: 0.4s;
      cursor: pointer;

      &:hover{
        background-color: white;
        color: #42B883;
      }
    }
  }

  .searchResult{
    color: rgba(0,0,0,0.8);
    max-width: 60vw;
    margin: 0 auto;
    
    @media only screen and (max-width: 800px){
      text-align: center;
      max-width: none;
    }
  }

  .movies-list {
    max-width: 60vw;
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(220px, 1fr));
    margin: 0 auto;
    margin-top: 30px;
    margin-bottom: 30px;
    justify-items: center;
    grid-gap: 30px;

    .movie {
      position: relative;
      border-radius: 5px;
      box-shadow: rgba(0, 0, 0, 0.1) 0px 10px 15px -3px, rgba(0, 0, 0, 0.05) 0px 4px 6px -2px;

      .movie-link {
        display: flex;
        flex-direction: column;
        height: 100%;

        .product-image {
          position: relative;
          display: block;
          height: 100%;
          
          img {
            border-radius: 5px;
            display: block;
            width: 100%;
            height: 100%;
            object-fit: cover;
          }

          .type {
            position: absolute;
            padding: 8px 16px;
            background-color: #42B883;
            color: #FFF;
            top: 16px;
            left: 0px;
            text-transform: capitalize;
          }

          .detail {
            background: linear-gradient(to bottom, transparent 0%, rgba(0,0,0,0.8) 50%);
            padding: 16px 10px;
            position: absolute;
            bottom: 0;
            width: 100%;
            border-radius: 5px;

            .year {
              color: #AAA;
              font-size: 14px;
            }

            h3 {
              color: #FFF;
              font-weight: 300;
              font-size: 18px;
            }
          }
        }
      }

      .overlay{
        background-color: rgba(0,0,0,0.8);
        width: 100%;
        height: 100%;
        position: absolute;
        display: flex;
        flex-direction: column;
        justify-content: center;
        align-items: center;
        color: white;
        opacity: 0;
        transition: 0.3s;
        border-radius: 3px;

        h3{
          padding: 10px;
          text-align: center;
        }

        &:hover{
          opacity: 1;
        }
      }
    }
  }
}
</style>