<template>
    <Loading v-if="$fetchState.pending" />
  <div v-else class="container single-movie">
      <NuxtLink class="button" :to="{ name: 'index'}">Back</NuxtLink>
      <div class="movie-info">
          <div class="movie-img">
              <img :src="`https://image.tmdb.org/t/p/w500/${movie.poster_path}`" alt="">
          </div>
          <div class="movie-content">
              <h1>Title: {{movie.title}}</h1>
              <p class="movie-fact tagline">
                  <span>Tagline: </span>{{movie.tagline}}
              </p>
              <p class="movie-fact">
                  <span>Released:</span>
                    {{
                        new Date(movie.release_date).toLocaleString('en-us', {
                        month: 'long',
                        day: 'numeric',
                        year: 'numeric',
                        })
                    }}
              </p>
              <p class="movie-fact">
                  <span>Duration: </span>{{movie.runtime}} minutes
              </p>
              <p class="movie-fact">
                  <span>Revenue:</span>
                  {{
                      movie.revenue.toLocaleString('en-us', {
                          style: 'currency',
                          currency: 'MYR'
                      })
                  }}
              </p>
              <p class="movie-fact">
                  <span>Overview: </span>{{movie.overview}}
              </p>
          </div>
      </div>
  </div>
</template>

<script>
import axios from 'axios'

export default {
  /* eslint-disable indent */
    name: 'SingleMovie',
    head () {
        return {
            title: `Movie Details - ${this.movie.title}`,
            meta: [
                {
                hid: 'description',
                name: 'description',
                content: 'Get all the latest streaming movies iin theaters & online'
                },
                {
                hid: 'keywords',
                name: 'keywords',
                content: 'movies, stream, streaming, online'
                }
            ]
        }
    },
    data () {
        return {
            movie: ''
        }
    },
    async fetch () {
        await this.getSingleMovie()
    },
    fetchDelay: 1000,
    methods: {
        async getSingleMovie () {
            const data = axios.get(`https://api.themoviedb.org/3/movie/${this.$route.params.movieid}?api_key=46b82d06a9b6224b0531c70bab82b005`)
            const result = await data
            this.movie = result.data
        }
    }
}
</script>

<style lang='scss' scoped>
.single-movie {
  color: #fff;
  min-height: 100vh;
  display: flex;
  flex-direction: column;
  justify-content: center;
  padding: 32px 16px;
  .button {
    align-self: flex-start;
    margin-bottom: 32px;
    text-decoration: none;
  }
  .movie-info {
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 32px;
    color: #fff;
    @media (min-width: 800px) {
      flex-direction: row;
      align-items: flex-start;
    }
    .movie-img {
      img {
        max-height: 500px;
        width: 100%;
        @media (min-width: 800px) {
          max-height: 700px;
          width: initial;
        }
      }
    }
    .movie-content {
      h1 {
        font-size: 56px;
        font-weight: 400;
      }
      .movie-fact {
        margin-top: 12px;
        font-size: 20px;
        line-height: 1.5;
        span {
          font-weight: 600;
          text-decoration: none;
        }
      }
      .tagline {
        font-style: italic;
        span {
          font-style: normal;
        }
      }
    }
  }
}
</style>
