<template>
      <div>
    <h1 class="text-center my-4">Lista de Juegos</h1>
    <div v-if="error" class="alert alert-danger text-center">{{ error }}</div>
    <div v-else-if="games.length === 0" class="text-center">Cargando juegos...</div>
    <div class="row">
      <div
        v-for="game in games"
        :key="game.id"
        class="col-12 col-md-4 mb-4 d-flex justify-content-center"
      >
        <!-- Card para cada juego -->
        <div class="card" style="width: 18rem;">
          <img
            :src="game.background_image"
            class="card-img-top"
            :alt="game.name"
          />
          <div class="card-body">
            <h5 class="card-title">{{ game.name }}</h5>
            <p class="card-text">Fecha de lanzamiento: {{ game.released }}</p>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
    name: 'component-games',
    // props: {},
    data: function(){
        return {
            games: [],
            error:null
        }
    },
    // computed: {},
    methods: {
        async fetchGames() {
            const apiKey = "d70f87bc2cd042f2873ec486e926d868";
            const url = `https://api.rawg.io/api/games?key=${apiKey}`;      
            try {
                const response = await fetch(url);
                if (!response.ok) {
                    throw new Error(`Error en la solicitud: ${response.status}`);//throw captura el error y detiene la ejecución pasandolo a el catch más cercano
                }
                const data = await response.json();
                this.games = data.results; // Almacena los juegos en la propiedad `games`
            } catch (error) {
                this.error = error.message; // Captura cualquier error
                console.error("Error al obtener los datos:", error);
            }
        }
    },
    // watch: {},
    // components: {},
    // mixins: [],
    // filters: {},
    // -- Lifecycle Methods
    created() {
    this.fetchGames(); // Llama al método al crear el componente
    },
    // -- End Lifecycle Methods
}
</script>

<style scoped>
    
</style>