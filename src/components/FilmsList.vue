<template>
    <div class='container'>
        <div class='row'>
            <film-filter v-model='search' :tags='tags' @tag='onTagUnFilterClicked'></film-filter>
        </div>
        <div id="cards" class='row'>
            <div class='col-3 card-container' v-for='film in filtered.slice(0,limit)' :key='film.id'>
                <!--card-->
                <film-card :card='film' @tag='onTagFilterClicked'></film-card>
            </div>
        </div>
        <div class='row' v-if='limit < filtered.length'>
            <div class='col text-center'>
                <button class='btn btn-primary' @click='limit+=4'>Еще</button>
            </div>
        </div>
        <div class='row' v-if='limit >=  filtered.length'>
            <div class='col'>
                <p class='text-center'>это всё</p>
            </div>
        </div>
        <div class='row'>
            <div class='col'>
                <p class='text-center' v-if='filtered.length==0'>Ничего не найдено</p>
            </div>
        </div>
    </div>
</template>

<script>
  import FilmFilter from './FilmFilter'
  import FilmCard from './FilmCard'
  import Axios from 'axios'

  export default {
    name: 'FilmsList',
    data () {
      return {
        search: '',
        films: [
          {
            id: 1,
            title: 'Трансформеры',
            desc: 'Роботы',
            tags: ['Фантастика', 'боевик']
          },
          {
            id: 2,
            title: 'Чаппи',
            desc: 'Искуственный интелект',
            tags: [
              'Техника', 'робот', 'ии'
            ]
          },
          {
            id: 3,
            title: 'Люси',
            desc: 'Фантастическая передозировка наркотиков',
            tags: ['Фантастика', 'боевик']
          },
          {
            id: 4,
            title: 'Трансформеры2',
            desc: 'Роботы',
            tags: ['Фантастика', 'боевик']
          },
          {
            id: 5,
            title: 'Чаппи2',
            desc: 'Искуственный интелект',
            tags: ['Техника', 'робот', 'ии']
          },
          {
            id: 6,
            title: 'Люси2',
            desc: 'Фантастическая передозировка наркотиков',
            tags: ['Фантастика', 'боевик']
          },
          {
            id: 6,
            title: 'Терминатор',
            desc: 'робот ищет мальца который закоротит его плату',
            tags: ['Фантастика', 'ии']
          }
        ],
        tags: [],
        limit: 4
      }
    },
    methods: {
      onTagFilterClicked (value) {
        if (!this.tags.includes(value)) {
          this.tags.push(value)
        }
      },
      onTagUnFilterClicked (value) {
        if (this.tags.includes(value)) {
          this.tags.splice(this.tags.indexOf(value), 1)
        }
      }
    },
    computed: {
      filtered () {
        let filter = this.search.toLowerCase().trim()
        let result = []

        if (filter === '') {
          result = this.films
        } else {
          result = this.films.filter(film => {
            if (film.title.toLowerCase().indexOf(filter) === 0) {
              return true
            }
          })
        }

        if (this.tags.length > 0) {
          return result.filter(item =>
            this.tags.every(tag => item.tags.includes(tag))
          )
        }

        return result
      }
    },
    components: { FilmFilter, FilmCard },
    created () {
      // TODO: сделать
      Axios.get('http://localhost:8080/static/films.json')
        .then(response => {
          this.films = response.data
        })
    }
  }
</script>