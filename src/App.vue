<template>
  <main class="container">
    <header class="header">
      <div class="hero">
        <div>
          <h1>Buscador de películas</h1>
          <p class="muted">Búsqueda, filtros y favoritos con persistencia en el navegador.</p>
        </div>

        <img class="hero-img" :src="heroImage" alt="Imagen decorativa" />
      </div>
    </header>

    <section class="panel">
      <input
        v-model.trim="q"
        class="input"
        type="text"
        placeholder="Busca por título (ej. 'devil', 'matrix')"
      />

      <select v-model="genre" class="select">
        <option value="all">Todos los géneros</option>
        <option v-for="g in genres" :key="g" :value="g">{{ g }}</option>
      </select>

      <label class="check">
        <input type="checkbox" v-model="onlyFavs" />
        Mostrar solo favoritos
      </label>
    </section>

    <section class="grid" v-if="filtered.length">
      <article class="card" v-for="m in filtered" :key="m.id">
        <div class="poster-wrap">
          <img class="poster" :src="m.image" :alt="`Poster de ${m.title}`" />
          <span v-if="isFav(m.id)" class="fav-badge">Favorito</span>
        </div>

        <div class="body">
          <div class="top">
            <h3 class="title">{{ m.title }}</h3>
            <p class="meta">{{ m.genre }} • {{ m.year }}</p>
          </div>

          <p class="desc">{{ m.desc }}</p>

          <div class="actions">
            <button
              class="btn"
              :class="isFav(m.id) ? 'btn-fav-on' : 'btn-fav-off'"
              @click="toggleFav(m.id)"
            >
              {{ isFav(m.id) ? "Quitar de favoritos" : "Agregar a favoritos" }}
            </button>
          </div>
        </div>
      </article>
    </section>

    <p v-else class="empty">No hay resultados con esos filtros.</p>

    <footer class="footer">
      <span>Resultados: {{ filtered.length }}</span>
      <span>Favoritos: {{ favs.size }}</span>
      <button class="btn btn-clear" @click="clearFavs" :disabled="favs.size === 0">
        Limpiar favoritos
      </button>
    </footer>
  </main>
</template>

<script setup>
import { computed, onMounted, ref } from "vue";

// Imagen decorativa del header
const heroImage =
  "https://img.freepik.com/foto-gratis/surtido-elementos-cine-sobre-fondo-rojo-espacio-copia_23-2148457848.jpg?semt=ais_user_personalization&w=740&q=80";

const movies = ref([
  {
    id: 1,
    title: "Devil May Cry (animación)",
    genre: "Acción",
    year: 2007,
    image:
      "https://m.media-amazon.com/images/M/MV5BMWZiMTVmMjEtYjQ4MS00YzE4LThmMGYtYTE3ZjhhZmQ2NmMwXkEyXkFqcGc@._V1_.jpg",
    desc: "Acción y demonios con estilo. Ejemplo para la app."
  },
  {
    id: 2,
    title: "The Matrix",
    genre: "Ciencia ficción",
    year: 1999,
    image:
      "https://biblioteca.ulpgc.es/sites/default/files/attachments_files/theend/Matrix.jpg",
    desc: "Realidad, simulación y acción con un toque filosófico."
  },
  {
    id: 3,
    title: "Inception",
    genre: "Ciencia ficción",
    year: 2010,
    image:
      "https://m.media-amazon.com/images/I/912AErFSBHL._AC_UF894,1000_QL80_.jpg",
    desc: "Un equipo entra en sueños para alterar ideas desde adentro."
  },
  {
    id: 4,
    title: "John Wick",
    genre: "Acción",
    year: 2014,
    image:
      "https://es.web.img3.acsta.net/pictures/14/10/01/14/18/135831.jpg",
    desc: "Acción intensa y coreografías de combate muy marcadas."
  },
  {
    id: 5,
    title: "Spirited Away",
    genre: "Animación",
    year: 2001,
    image:
      "https://m.media-amazon.com/images/M/MV5BNTEyNmEwOWUtYzkyOC00ZTQ4LTllZmUtMjk0Y2YwOGUzYjRiXkEyXkFqcGc@._V1_.jpg",
    desc: "Aventura en un mundo fantástico con estética muy reconocible."
  }
]);

const q = ref("");
const genre = ref("all");
const onlyFavs = ref(false);

// Favoritos (persisten en localStorage)
const favs = ref(new Set());

function loadFavs() {
  const saved = localStorage.getItem("favs");
  if (!saved) return;
  try {
    const arr = JSON.parse(saved);
    favs.value = new Set(Array.isArray(arr) ? arr : []);
  } catch {
    favs.value = new Set();
  }
}

function saveFavs() {
  localStorage.setItem("favs", JSON.stringify([...favs.value]));
}

function toggleFav(id) {
  if (favs.value.has(id)) favs.value.delete(id);
  else favs.value.add(id);
  saveFavs();
}

function isFav(id) {
  return favs.value.has(id);
}

function clearFavs() {
  favs.value = new Set();
  saveFavs();
}

onMounted(loadFavs);

const genres = computed(() => {
  const set = new Set(movies.value.map(m => m.genre));
  return [...set].sort();
});

const filtered = computed(() => {
  const term = q.value.toLowerCase();
  return movies.value
    .filter(m => genre.value === "all" || m.genre === genre.value)
    .filter(m => !term || m.title.toLowerCase().includes(term))
    .filter(m => !onlyFavs.value || favs.value.has(m.id));
});
</script>

<style scoped>
/* Fondo oscuro general */
.container {
  max-width: 1050px;
  margin: 40px auto;
  padding: 0 16px;
  font-family: Arial, sans-serif;
  color: #f8fafc;
}

/* Header */
.header { margin-bottom: 16px; }

.hero {
  display: flex;
  gap: 16px;
  align-items: center;
  justify-content: space-between;
}

.hero-img {
  width: 240px;
  height: 120px;
  object-fit: cover;
  border-radius: 14px;
  border: 1px solid rgba(255,255,255,0.14);
}

h1 { margin: 0; }

.muted {
  color: rgba(241,245,249,0.65);
  margin-top: 8px;
}

/* Panel de filtros: ya no blanco */
.panel {
  display: grid;
  gap: 10px;
  grid-template-columns: 1fr 220px 220px;
  align-items: center;
  padding: 14px;
  border: 1px solid rgba(255,255,255,0.14);
  border-radius: 12px;
  background: rgba(255,255,255,0.06);
  backdrop-filter: blur(6px);
}

/* Inputs oscuros */
.input, .select {
  width: 100%;
  padding: 10px 12px;
  border: 1px solid rgba(255,255,255,0.18);
  border-radius: 10px;
  background: rgba(0,0,0,0.25);
  color: #f1f5f9;
}

.input::placeholder {
  color: rgba(241,245,249,0.6);
}

.check {
  display: flex;
  gap: 8px;
  align-items: center;
  user-select: none;
  color: rgba(241,245,249,0.85);
}

/* Grid */
.grid {
  margin-top: 16px;
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 14px;
}

/* Cards: ya no blanco */
.card {
  border: 1px solid rgba(255,255,255,0.14);
  border-radius: 14px;
  overflow: hidden;
  display: grid;
  grid-template-columns: 140px 1fr;
  background: rgba(255,255,255,0.06);
  backdrop-filter: blur(6px);
}

/* Poster */
.poster-wrap {
  position: relative;
  width: 140px;
  background: rgba(0,0,0,0.15);
}

.poster {
  width: 140px;
  height: 100%;
  object-fit: cover;
  display: block;
}

/* Badge favorito */
.fav-badge {
  position: absolute;
  top: 10px;
  left: 10px;
  padding: 5px 10px;
  border-radius: 999px;
  background: #ffcc00;
  color: #1a1a1a;
  font-size: 0.8rem;
  font-weight: 700;
  border: 1px solid rgba(0,0,0,0.15);
}

/* Texto dentro de card */
.body { padding: 12px; display: grid; gap: 10px; }

.title { margin: 0; font-size: 1.05rem; color: #f8fafc; }
.meta { margin: 6px 0 0; color: #cbd5e1; font-size: 0.9rem; }
.desc { margin: 0; color: #e5e7eb; font-size: 0.95rem; }

.actions { display: flex; gap: 10px; }

/* Botones */
.btn {
  padding: 10px 12px;
  border-radius: 10px;
  cursor: pointer;
  border: 1px solid transparent;
  font-weight: 700;
}

/* Favoritos: color visible */
.btn-fav-off {
  background: #2563eb;
  color: white;
}
.btn-fav-off:hover { background: #1d4ed8; }

.btn-fav-on {
  background: #ef4444;
  color: white;
}
.btn-fav-on:hover { background: #dc2626; }

.btn-clear {
  background: rgba(255,255,255,0.08);
  border: 1px solid rgba(255,255,255,0.14);
  color: #f8fafc;
}
.btn-clear:hover { background: rgba(255,255,255,0.12); }

.btn:disabled { opacity: 0.6; cursor: not-allowed; }

/* Footer */
.footer {
  margin-top: 18px;
  display: flex;
  justify-content: space-between;
  gap: 12px;
  align-items: center;
  padding: 12px 0;
  border-top: 1px solid rgba(255,255,255,0.12);
  color: #e5e7eb;
}

.empty {
  margin-top: 18px;
  color: rgba(241,245,249,0.65);
  font-style: italic;
}

/* Responsive */
@media (max-width: 900px) {
  .grid { grid-template-columns: repeat(2, 1fr); }
  .panel { grid-template-columns: 1fr; }
  .hero { flex-direction: column; align-items: flex-start; }
  .hero-img { width: 100%; height: 160px; }
}

@media (max-width: 520px) {
  .grid { grid-template-columns: 1fr; }
  .card { grid-template-columns: 120px 1fr; }
  .poster-wrap, .poster { width: 120px; }
}
</style>