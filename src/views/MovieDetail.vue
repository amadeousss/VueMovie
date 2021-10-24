<template>
    <header>
        <router-link :to="'../'">
        <h1><span>Vue</span>Movies</h1>
        </router-link>
    </header>
    <div class="movie-detail">
        <div>
            <h1 class="image-title">{{ movie.Title }}</h1>
            <img :src="movie.Poster"  class="featured-image">
        </div>
        <div class="text-details">
            <h2>{{ movie.Title }}</h2>
            <div>
                <p>Director</p>
                <p>{{ movie.Director }}</p>
            </div>
            <div>
                <p>Country</p>
                <p>{{ movie.Country }}</p>
            </div>
            <div>
                <p>Genre</p>
                <p>{{ movie.Genre }}</p>
            </div>
            <div>
                <p>Released</p>
                <p>{{ movie.Released }}</p>
            </div>
            <div>
                <p>Runtime</p>
                <p>{{ movie.Runtime }}</p>
            </div>
            <div>
                <p>Rated</p>
                <p>{{ movie.Rated }}</p>
            </div>
        </div>
    <div class="plot">
        <h3>Plot</h3>
        <p id="plot">{{ movie.Plot }}</p>
    </div>
    </div>
</template>

<script>
import { ref, onBeforeMount } from 'vue';
import { useRoute } from 'vue-router';
import env from '@/env.js';

export default{
    setup(){
        const movie = ref({});
        const route = useRoute();

        onBeforeMount(() => {
            fetch(`https://www.omdbapi.com/?apikey=${env.apikey}&i=${route.params.id}&plot=full`)
            .then(response => response.json())
            .then(data => {
                movie.value = data;
            })
        }) ;

        return {
            movie
        }
    }
}
</script>

<style lang="scss">
header{
    h1{
        color: rgba(0,0,0,0.8);
    }
}

.movie-detail{
    padding: 50px;
    display: grid;
    grid-template-columns: 1fr 1.5fr;
    grid-gap: 30px;
    max-width: 1100px;
    margin: 0 auto;

    .image-title{
        display: none;
    }

    .featured-image{
        display: block;
        border-radius: 5px;
        box-shadow: 0 0 5px 2px rgba(0,0,0,0.3);
    }

    .text-details{

        h2{
            font-size: 3em;
            margin: -12px 12px 0 0;
            color: rgba(0,0,0,0.8);
            font-weight: 800;
        }
        
        p{
        font-family: 'Roboto';
        font-weight: 800;
        color: rgba(0,0,0,0.8);
        font-size: 16px;
        line-height: 2;

            &#plot{
                font-weight: 600;
            }
        }

        div{
            display: grid;
            grid-template-columns: 100px 1fr;
            letter-spacing: 2px;
            margin: 10px 0 0 2px;
            :first-child{
                color: rgba(0,0,0,0.6);
                font-weight: 500;
            }
        }
    }

}

.plot{
    grid-column: 1/3;
    margin: 20px auto;

    h3{
        color: rgba(0,0,0,0.8);
        text-align: center;
        font-size: 2.5rem;
    }

    p{
        font-size: 1.5rem;
        text-align: justify;
        line-height: 4rem;
        font-weight: 500;
        font-family: 'Roboto';
    }

}

@media only screen and(max-width: 1200px){
    .movie-detail{
        max-width: 800px;
    }
}

@media only screen and (max-width:860px){
    .movie-detail{
        grid-template-columns: 1fr;
        justify-items: center;
        grid-column-gap: 0;
        padding: 20px;

        .image-title{
            display: block;
            font-size: 3em;
            margin-bottom: 20px;
            color: rgba(0,0,0,0.8);
            font-weight: 800;
            text-align: center;
        }

        .text-details{
            grid-row: 2;
            max-width: 300px;
            h2{
                display: none;
            }
            div{
                grid-template-columns: 1fr 1fr;
                grid-column-gap: 10px;
                :nth-child(1){
                    text-align: left;
                }
                :nth-child(2){
                    text-align: right;
                }
            }
        }

        .plot{
            grid-row: 3;
            max-width: 300px;

            h3{
                margin-bottom: 10px;
            }

            p{
                font-size: 1.2rem;
                line-height: 2.7rem;
            }
        }
    }
}
</style>
