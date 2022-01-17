<template lang="pug">
.beer
  img(src="img/beer-bar.jpg" width="500" height="500")
  .beer__previous
    h2 Last 3 beers
    ul.beer__previous-list
      li(v-for="(value,index) in previousBeer" v-on:click="show" )
        img(:src='`${value.image}`' width="100" height="200" :class='`${index}`')
  .beer__container
    h1 {{ title }}
    .beer__wrapper
      span.preloader
        span.preloader__center
      .beer__image
        img(:src='`${beerImage}`' width="150" height="400")
      .beer__info
        p.beer__name {{ beerName }}
        ul.beer__info-list
          li.beer__item-info Alcohol: 
            span {{ alcohol }}
          li.beer__item-info Blg: 
            span {{ blg }}
          li.beer__item-info Brand: 
            span {{ brand }}
          li.beer__item-info Hop: 
            span {{ hop }}
          li.beer__item-info Ibu: 
            span {{ ibu }}
          li.beer__item-info Malts: 
            span {{ malts }}
          li.beer__item-info Yeast: 
            span {{ yeast }}
    button(v-on:click="change").beer__change {{ buttonTitle}}
</template>

<script>
import axios from 'axios'

export default {
  name: 'Beer',
  data () {
    return { 
      beerName: null, 
      alcohol: null,
      blg: null,
      brand: null,
      hop: null,
      ibu: null,
      malts: null,
      yeast: null,
      errored: false,
      beerImage: null,
      title: null,
      buttonTitle: 'Get beer!',
      previousBeer: [],
      previousBeerItem: {},
    }
  },
  methods: {
    change: function(evt) {
      evt.preventDefault();
      const beerWrapper = document.querySelector('.beer__wrapper');
      const previousList = document.querySelector('.beer__previous');
      if (!(this.previousBeer.length === 0)) {
        if (!previousList.classList.contains('visible')) {
          previousList.classList.add('visible');
        }
      }
      this.previousBeerItem.image = this.beerImage;
      this.previousBeerItem.name = this.beerName;
      this.previousBeerItem.alcohol = this.alcohol;
      this.previousBeerItem.blg = this.blg;
      this.previousBeerItem.brand = this.brand;
      this.previousBeerItem.hop = this.hop;
      this.previousBeerItem.ibu = this.ibu;
      this.previousBeerItem.malts = this.malts;
      this.previousBeerItem.yeast = this.yeast;
      let copyObject = {};
      Object.assign(copyObject, this.previousBeerItem);
      this.previousBeer.push(copyObject);
      console.log(this.previousBeer);
      if (!beerWrapper.classList.contains('visible')) {
        beerWrapper.classList.add('visible');
      }   
      if(this.previousBeer.length <= 4) {
        console.log(this.previousBeer.length)
      } else {
        this.previousBeer = this.previousBeer.slice((this.previousBeer.length - 4), this.previousBeer.length);
      }
      this.title = 'Your beer for today!';
      this.buttonTitle = 'New Beer!';
      axios
        .get('https://random-data-api.com/api/beer/random_beer')
        .then(response => (
          this.beerName = response.data.name,
          this.alcohol = response.data.alcohol,
          this.blg = response.data.blg,
          this.brand = response.data.brand,
          this.hop = response.data.hop,
          this.ibu = response.data.ibu,
          this.malts = response.data.malts,
          this.yeast = response.data.yeast
          ))
        .catch(error => {
          console.log(error);
          this.errored = true;
        })
        .finally(() => (this.loading = false));
      axios
        .get('https://api.punkapi.com/v2/beers')
        .then(response => (
          this.beerImage = response.data[Math.floor(Math.random() * (response.data.length - 1)) + 1].image_url
          ))
        .catch(error => {
          console.log(error);
          this.errored = true;
        })
        .finally(() => {
          this.loading = false;
          const beerWrapper = document.querySelector('.beer__wrapper');
          beerWrapper.classList.add('loaded');
          });
    },
    show: function(evt) {
      evt.preventDefault();
      let index = evt.target.classList.value;
      this.beerImage = this.previousBeer[index].image,
      this.beerName = this.previousBeer[index].name,
      this.alcohol = this.previousBeer[index].alcohol,
      this.blg = this.previousBeer[index].alcohol,
      this.brand = this.previousBeer[index].brand,
      this.hop = this.previousBeer[index].hop,
      this.ibu = this.previousBeer[index].ibu,
      this.malts = this.previousBeer[index].malts,
      this.yeast = this.previousBeer[index].yeast
    },
  },
}
</script>
