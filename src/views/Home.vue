<template>
  <div class="home">
      
    <div class="feature-card">
      <router-link to="/movie/tt0409591">
        <img src="https://th.bing.com/th/id/OIP.TUyjS6Wc4-AMFo8ic3kwhAHaDt?w=323&h=174&c=7&o=5&dpr=1.25&pid=1.7" alt="Naruto Poster" class="featured-img" />
        <div class="detail">
          <h3>Transformers</h3>
          <p>Transformers Series, started out in mid 1980's is a story of
            conflict between righteous Autobots, under the leadership of
            Optimus Prime, and evil Decepticons with Megatron as their leader.
            The fierce war destroyed Cybertron, their home and they came Earth
            as refugees, giving life to another war....
          </p>
        </div>
      </router-link>
    </div>
    <form @submit.prevent="SearchMovies()" class="search-box">
      <input type="text" placeholder="Search for a movie" v-model="search">
      <input type="submit" value="Search">
    </form>
    
    <div class="movies-list">
      <div class="movie" v-for="movie in movies" :key="movie.imbID">
        <router-link :to="'/movie/' + movie.imdbID" class="movie-link">
        <div class="product-image">
          <img :src="movie.Poster" alt="Movie Poster" />
          <div class="type">{{ movie.Type }}</div>
        </div>
        <div class="detail">
          <p class="year">{{ movie.Year }}</p>
          <h3>{{ movie.Title }}</h3>
        </div>
        </router-link>
      </div>   
    </div>

  </div>
</template>

<script>
import { ref } from 'vue';
import env from '@/env.js'
export default {
  setup () {
    const search = ref("");
    const movies = ref([]);
    const SearchMovies = () => {
      if (search.value != "") {
        fetch(`http://www.omdbapi.com/?apikey=${env.api_key}&s=${search.value}`)
          .then(response => response.json())
          .then(data => {
            movies.value = data.Search;
            search.value = "";
          });
      }
    }
    return {
      search,
      movies,
      SearchMovies
    }
  }
}
</script>

<style lang="scss">

.home {

  .feature-card {
    position: relative;
    max-width: 1000px;
    margin: auto;

    .featured-img {
      display: block;
      width: 100%;
      max-width: 1200px;
      margin: auto;
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
        color:#FFF;
        margin-bottom: 16px;
      }

      p {
        color: #FFF;
      }

    }
  }
}

.search-box {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  padding: 16px;

  input {
    display: block;
    appearance: none;
    border: none;
    outline: none;
    background: none;

    &[type="text"] {
      width: 100%;
      color: #fff;
      transition: 0.4s;
      background-color: #496583;
      font-size: 20px;
      padding: 10px 16px;
      border-radius: 8px;
      margin-bottom: 25px;
      max-width: 1000px;
      margin: auto;
    
      &::placeholder {
        color: #f3f3f3;
      }

      &:focus {
        box-shadow: 0px 3px 6px rgba(0, 0, 0, 0.6);
      }
    }

    &[type="submit"] {
      width: 100%;
      max-width: 300px;
      background-color: #42B883;
      padding: 8px;
      border-radius: 8px;
      color: #fff;
      font-size: 20px;
      margin-top: 15px;
      height: 50px;
      text-transform: uppercase;
      transition: 0.4s;

      &:active {
        background-color: #3b8070;
      }

    }
  }
}

.movies-list {
  display: flex;
  flex-wrap: wrap;
  margin: 0px 8px;

  .movie {
    max-width: 50%;
    flex: 1 1 50%;
    padding: 16px 8px;
  }

  .movie-link {
    display: flex;
    flex-direction: column;
    height: 100%;

    .product-image {
      position: relative;
      display: block;

      img {
        display: block;
        width: 100%;
        height: 375px;
        object-fit: cover;
      }

      .type {
        position: absolute;
        padding: 8px 16px;
        background-color: #42B883;
        color: #FFF;
        bottom: 16px;
        left: 0px;
        text-transform: capitalize;
      }
    }

    .detail {
      background-color: #496583;
      padding: 16px 8px;
      flex: 1 1 100%;
      border-radius: 0px 0px 8px 8px;

      .year {
        color: #AAA;
        font-size: 14px;
      }

      h3 {
        color: #FFF;
        font-weight: 600;
        font-size: 18px;
      }
    }
  }
}

</style>
