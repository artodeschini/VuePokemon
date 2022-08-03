<template>
    <div id="pokemon"> 
        <div class="card">
        <div class="card-image">
            <figure>
                <img :src="currentImg" alt="Placeholder image">
            </figure>
        </div>
        <div class="card-content">
            <div class="media">
            <!-- <div class="media-left">
                <figure class="image is-48x48">
                <img :src="pokemon.front" alt="Placeholder image">
                </figure>
            </div>
            <br> -->
            <div class="media-content">
                <p class="title is-4">{{ num }} {{ upperName }}</p>
                <p class="subtitle is-6">{{ pokemon.type }}</p>
            </div>
            </div>

            <div class="content">
            <!-- Lorem ipsum dolor sit amet, consectetur adipiscing elit.
            Phasellus nec iaculis mauris. <a>@bulmaio</a>. -->
                {{ pokemon.ability }}
            <!-- <a href="#">#css</a> <a href="#">#responsive</a>
            <br>
            <time datetime="2016-1-1">11:09 PM - 1 Jan 2016</time> -->
            <br>
            <br>
            <button class="button is-medium is-fullwidth is-warning" @click="mudarImagem">Girar</button>
            </div>
        </div>
        </div>
    </div>
</template>
<script>
import axios from 'axios';
export default {
    name: 'ListPokemons',
    props: {
        num: Number,
        name: String,
        url: String
    },
    computed: {
      upperName() {
        let newName = this.name[0].toUpperCase() + this.name.slice(1);
        return newName;
      }
    }, 
    created: function() {
        axios.get(this.url).then(res => {
            //console.log("pegando a lista de pokemons");
            //this.pokemons = res.data.results;
            // console.log(res);
            this.pokemon.type = res.data.types[0].type.name;
            this.pokemon.front = res.data.sprites.front_default;
            this.pokemon.back = res.data.sprites.back_default;
            this.pokemon.ability = res.data.abilities[0].ability.name;
            this.currentImg = this.pokemon.front;
            this.isFront = true;
        });
    },
    data() {
        return {
            isFront: true,
            currentImg: '',
            pokemon: { type: '', front: '', back: '', ability: ''}
        }
    },
    methods: {
        mudarImagem: function() {
            if (this.isFront) {
                this.isFront = false;
                this.currentImg = this.pokemon.back;
            } else {
                this.isFront = true;
                this.currentImg = this.pokemon.front;
            }
        }
    }
}
</script>
<style scoped>

#pokemon {
    margin-top: 2%;
}

</style>