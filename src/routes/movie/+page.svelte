<!--
 tits-mp - Management page for unofficial TIT&S website (github.com/WeebDeveloperz/titsunofficial)
 Copyright (C) 2023  titsunofficial contributors

 This program is free software: you can redistribute it and/or modify
 it under the terms of the GNU General Public License as published by
 the Free Software Foundation, either version 3 of the License, or
 (at your option) any later version.

 This program is distributed in the hope that it will be useful,
 but WITHOUT ANY WARRANTY; without even the implied warranty of
 MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
 GNU General Public License for more details.

 You should have received a copy of the GNU General Public License
 along with this program.  If not, see <https://www.gnu.org/licenses/>.
-->

<script>
  import { fade } from 'svelte/transition';
  import { onMount } from 'svelte';
  import { BASE_URL } from "../../config.js"
  import Fa from 'svelte-fa/src/fa.svelte'
  import { faArrowLeft, faRotateRight } from '@fortawesome/free-solid-svg-icons'

  let username;
  let password = "";
  let noticeText = "";

  let movies = [];
  const loadMovies = () =>
    fetch(BASE_URL + 'movies')
      .then(response => response.json())
      .then(data => movies = data.data);

  onMount(async () => {
    if (!sessionStorage.getItem("authtoken")) {
      window.location.replace("/");
    }
    loadMovies();
  })

  const filterMovies = (movies, searchString) => movies;

  let searchString = "";
</script>

<div class="back-button">
  <a href="/home">
    <Fa icon={faArrowLeft} />
  </a>
</div>

<div class="searchbox">
  Search Movies (disabled temporarily) <input bind:value={searchString}/>
</div>

<div class="reload-button">
  <a href="" on:click={loadMovies}>
    <Fa icon={faRotateRight}/>
  </a>
</div>

{#key movies}
<table in:fade="{{ duration: 200, delay: 100 }}" out:fade="{{ duration: 300, delay: 0 }}">
  <tr>
    <th>College ID</th>
    <th>Student Name</th>
    <th>Movie Title</th>
  </tr>
  {#if movies != []}
    {#each filterMovies(movies, searchString) as m}
      <tr>
        <td>{m.submitter_college_id}</td>
        <td>{m.submitter_name}</td>
        <td>{m.movie_title}</td>
        <button on:click={handleDelete}>Delete</button>
      </tr>
    {:else}
      <h1>No Movies With This Filter Found.</h1>
    {/each}
  {:else}
    <h1>No Movies Exist In The Database.</h1>
  {/if}
</table>
{/key}

<style>
  * {
    font-family: sans-serif;
  }
  .login-page {
    color: white;
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    height: 95vh;
  }
  input {
    background-color: #232627;
    border: 1px solid #dfdfdf;
    padding: 0.3rem;
    font-size: 0.9em;
    outline: 0;
    border-radius: 4px;
    color: white;
    box-sizing: border-box;
  }
  input:focus {
    border: 1px solid #b481da;
  }
  input::placeholder {
    color: gray;
  }
  button {
    border-radius: 4px;
    border: 1px solid #b481da;
    background-color: #232627;
    color: white;
    transition: background-color 0.2s, color 0.2s;
    width: 7rem;
  }
  button:hover, button:focus {
    outline: 0;
    background-color: #b481da;
    color: #232627;
  }
  .button-wrapper {
    width: 30rem;
    display: flex;
    justify-content: flex-end;
  }
  .fields {
    position: relative;
  }
  .field {
    width: 30rem;
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin: 0.5rem 0;
  }
  .field input, .field select {
    width: 60%;
  }
  .notice {
    position: absolute;
    bottom: -3rem;
    text-align: center;

    left: 0; right: 0;
    margin: auto;
  }
  .warning {
    color: red;
  }
  button {
    width: 6rem;
    height: 2rem;
  }
  .searchbox {
    width: 100%;
    display: flex;
    justify-content: center;
    align-items: center;
    margin: 2rem 0;
    color: white;
  }
  .searchbox input {
    margin-left: 1rem;
  }
  table {
    width: 80%;
    margin: auto;
    color: white;
  }
  th, td {
    text-align: left;
    border-bottom: 1px solid gray;
  }
  .back-button, .reload-button {
    font-size: 2rem;
    position: fixed;
    top: 1rem;
  }
  .back-button a, .reload-button a {
    color: white;
    text-decoration: none;
  }
  .back-button a:hover,
  .reload-button a:hover {
    color: #b481da;
  }
  .back-button { left: 1rem; }
  .reload-button { right: 1rem; }
</style>
