<template>

  <div id="sobre">
    <h3>Personagens</h3>

    <div class="card-container">
      <div v-for="personagem in personagens" :key="personagem.id" class="card">
        <div class="card-image">
          <img :src="personagem.thumbnail.path+'.'+ personagem.thumbnail.extension" :alt="personagem.name" />
        </div>
        <div class="card-content">
          <p>{{ personagem.name }}</p>
            <p>{{ personagem.description }}</p>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import md5 from 'md5'
import axios from 'axios'

export default {
  name: 'sobre-view',

  data() {
    return {
      ts: Date.now(),
      chave_publica: 'ad47d7f1f3a2a985a9a732f6ca5f9c3e',
      chave_privada: 'a2b53807860290c492bb1757ebb2eac0ef8acfdd',
      hash: '',
      personagens: [],
    }
  },

  mounted() {
    this.hash = md5(this.ts + this.chave_privada + this.chave_publica);
    const url = `http://gateway.marvel.com/v1/public/characters?ts=${this.ts}&apikey=${this.chave_publica}&hash=${this.hash}`;

    axios.get(url)
      .then(res => {
        console.log(res);
        this.personagens = res.data.data.results;
      });
  },

}


</script>


<style>


p{
  color: aliceblue;
}
body {
  background-color: rgb(81, 94, 99);
  color: white;
}

.card-container {
  display: flex;
  flex-wrap: wrap;
  justify-content: center; 
  align-items: center; 
  margin-top: 20px; 
}


.card {
  width: 300px;
  border-radius: 8px;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.2);
  margin-right: 20px;
  margin-bottom: 20px;
  position: relative;
  overflow: hidden;
}

.card::before {
  content: '';
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-image: url('comic.png');
  opacity: 0.2;
}

.card:last-child {
  margin-right: 0;
}

.card-image {
  border-top-left-radius: 8px;
  border-top-right-radius: 8px;
  overflow: hidden;
}

.card-image img {
  width: 100%;
  height: auto;
}

.card-content {
  padding: 20px;
}

.title.is-4 {
  font-size: 1.5rem;
  font-weight: bold;
}
</style>