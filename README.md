# Buscador de Películas - Vue.js

Aplicación desarrollada con Vue 3 utilizando Vite como entorno de desarrollo.  
Permite buscar películas, filtrarlas por género y marcarlas como favoritas con persistencia en el navegador mediante localStorage.

---

## Descripción

Este proyecto consiste en una aplicación web interactiva construida con Vue.js.  
El usuario puede:

- Buscar películas por título.
- Filtrar por género.
- Mostrar únicamente películas marcadas como favoritas.
- Agregar o quitar películas de favoritos.
- Mantener los favoritos guardados incluso al recargar la página.

El objetivo principal fue practicar el uso de:

- Reactividad con `ref` y `computed`
- Renderizado dinámico con `v-for`
- Enlaces bidireccionales con `v-model`
- Manejo de eventos con `@click`
- Persistencia usando `localStorage`
- Diseño responsive con CSS

---

## Tecnologías utilizadas

- Vue 3
- Vite
- JavaScript (ES6)
- HTML5
- CSS3

---

## Estructura del proyecto
```text
buscador-vue/
├── index.html
├── package.json
├── vite.config.js
├── src/
│ ├── main.js # Punto de entrada de Vue
│ ├── App.vue # Componente principal
│ └── assets/ # Recursos estáticos
└── node_modules/ # Dependencias instaladas
```

---

## Instalación y ejecución

1. Instalar dependencias:
npm install

2. Ejecutar en modo desarrollo:
npm run dev

3. Abrir en el navegador la URL que aparece en la terminal  
(normalmente `http://localhost:5173`).

---

## Funcionalidades principales

- Búsqueda en tiempo real.
- Filtrado por género.
- Sistema de favoritos.
- Indicador visual de favoritos.
- Persistencia de datos en el navegador.
- Interfaz adaptable a dispositivos móviles.

---

## Persistencia

Los favoritos se almacenan en `localStorage`, por lo que permanecen guardados aunque el usuario recargue la página.

---

## Autor
Jonathan Alexis Lozano Figueroa
Proyecto realizado como práctica de desarrollo con Vue.js.
