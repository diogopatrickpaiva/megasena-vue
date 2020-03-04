<template>
  <div class="container-fluid">    
    <div id="buttons" class="row">
      <div class="col-md-1">
        <button type="button" class="btn btn-primary" v-on:click="getRandomNumber" :disabled="buttonStartDisable">Iniciar sorteio</button>
      </div>

      <div class="col-md-2">
        <button type="button" class="btn btn-info" v-on:click="getRandomNumber" :disabled="buttonGenerateDisable">Gerar novos números</button>
      </div>
    </div>
    
    <div id="result" class="row">
      <div class="col-md-12 subtitle">
          Sorteio:
      </div>

      <div class="col-md-12">
        <div v-if="start">
          <span class="badge badge-dark">Clique no botão "Novo Jogo" e após adicionar um ou mais jogos clique no botão "Iniciar sorteio".</span>
        </div>

        <div v-else class="number number-result" v-for="number in numberSort" v-bind:key="number.id">
          {{ number }}
        </div>
      </div>
    </div>

    <div id="games" class="row margin-double-row">
      <div class="col-md-12">
        <h1>
          Meus jogos
        </h1>
      </div>

      <div class="col-md-12" v-for="(value, name) in numberGames" v-bind:key="value.id">
        <div class="row">
          <div class="title-game">
            {{ name + 1 }}:
          </div>
          <div class="number" v-for="(value) in value" v-bind:key="value.id">
            {{ value }}
          </div>
        </div>
      </div>      
    </div>

    <div id="reset" class="row margin-double-row">
      <div class="col-md-2">
        <button type="button" class="btn btn-primary" data-toggle="modal" data-target="#exampleModal">Novo jogo</button>
      </div>
    </div>

    <!-- Modal -->
    <div class="modal fade" id="exampleModal" tabindex="-1" role="dialog" aria-labelledby="exampleModalLabel" aria-hidden="true">
      <div class="modal-dialog modal-lg" role="document">
        <div class="modal-content">
          <div class="modal-header">
            <h5 class="modal-title" id="exampleModalLabel">Novo jogo</h5>
            <button type="button" class="close" data-dismiss="modal" aria-label="Close">
              <span aria-hidden="true">&times;</span>
            </button>
          </div>

          <div class="modal-body">
            <div class="row">
              <div class="col-md-2">
                <input type="number" name="numberGame" min="1" max="60" v-model="value1">
              </div>

              <div class="col-md-2">
                <input type="number" name="numberGame" min="1" max="60" v-model="value2">
              </div>

              <div class="col-md-2">
                <input type="number" name="numberGame" min="1" max="60" v-model="value3">
              </div>

              <div class="col-md-2">
                <input type="number" name="numberGame" min="1" max="60" v-model="value4">
              </div>

              <div class="col-md-2">
                <input type="number" name="numberGame" min="1" max="60" v-model="value5">
              </div>

              <div class="col-md-2">
                <input type="number" name="numberGame" min="1" max="60" v-model="value6">
              </div>
            </div>
          </div>
          
          <div class="modal-footer">
            <button type="button" class="btn btn-secondary" data-dismiss="modal">Fechar</button>
            <button type="button" class="btn btn-primary" data-dismiss="modal" v-on:click="insertGame">Salvar</button>
          </div>
        </div>
      </div>
    </div>

  </div>
</template>

<script>
  // @ is an alias to /src
  // import HelloWorld from '@/components/HelloWorld.vue'

  export default {
    name: 'Home',  
    components: {
      // HelloWorld,
    },
    data: function () {
      return {
        start: true,
        buttonStartDisable: true,
        buttonGenerateDisable: true,
        min: 1,
        max: 60,
        numberSort: [],
        numberResults: [],
        numberValues: [],
        numberGames: [],
        value1: '',
        value2: '',
        value3: '',
        value4: '',
        value5: '',
        value6: '',
      }
    },
    methods: {
      loadNum: function () {
        this.min = 0;
        this.max = 60;
        this.getRandomNumber();
      },
      getInput: function () {
        let min = Number(this.min)
        let max = Number(this.max)

        if(min > max) {
          [min, max] = [max, min]
        }

        this.min = min
        this.max = max
        this.getRandomNumber()
      },
      getRandomNumber: function () {
        this.start = false;
        this.buttonStartDisable = true;
        this.buttonGenerateDisable = false;

        this.numberSort = this.generateNumber();
      },
      generateNumber: function () {
        return [...Array(6)].map(() => Math.floor(Math.random() * 60));
      },
      insertGame: function() {
        this.numberValues = [this.value1, this.value2, this.value3, this.value4, this.value5, this.value6];

        this.numberGames.push(this.numberValues);

        this.value1 = '',
        this.value2 = '',
        this.value3 = '',
        this.value4 = '', 
        this.value5 = '',
        this.value6 = ''
        
        this.buttonStartDisable = false;        
      },
    }
  }
</script>

<style>
  .row {
    margin-top: 1rem;
    margin-bottom: 1rem;
  }

  .margin-double-row {
    margin-top: 2rem;
  }

  .btn-primary, .btn-info {
    font-weight: bold;
  }

  h1 {
    font-weight: bold;
    font-size: 2rem;
    text-transform: uppercase;
  }
  
  .subtitle {
    font-weight: bold;
    text-transform: uppercase;
  }

  .number {
    height: 3.5rem;
    width: 3.5rem;
    background-color: rgb(28, 117, 168);
    color: #ffffff;
    font-weight: bold;
    font-size: 2rem;
    padding-top: 0.05rem;
    text-align: center;
    border-radius: 50%;
    border: solid 2px #075f6e;
    display: inline-block;
    margin-top: 0.5rem;
    margin-right: 1rem;
  }

  .number-result {
    background-color: rgb(23, 196, 101);
    border: solid 2px rgb(27, 175, 93);
  }

  .badge-dark {
    font-size: 1rem;
    padding: 1rem;
  }

  .title-game {
    padding-top: 0.6rem;
    padding-left: 1rem;
    margin-right: 1rem; 
    font-size: 2rem;
    font-weight: bold;
  }
</style>