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
  import { onMount } from 'svelte';
  onMount(_ => {
    if (!sessionStorage.getItem("authtoken")) {
      window.location.replace("/");
    }
  });
</script>

<div class="menu">
  <a><button on:click={_ => window.location.replace("/index-img")}>Upload Post</button></a>
  <a><button on:click={_ => window.location.replace("/subjects")}>Subjects</button></a>
  <a><button on:click={_ => window.location.replace("/files")}>Files</button></a>
</div>

<style>
  * {
    font-family: sans-serif;
  }
  button {
    border-radius: 4px;
    border: 1px solid #b481da;
    background-color: #232627;
    color: white;
    transition: background-color 0.2s, color 0.2s;
  }
  button:hover, button:focus {
    outline: 0;
    background-color: #b481da;
    color: #232627;
  }
  .menu {
    position: fixed;
    margin: auto;
    top: 0; bottom: 0;
    left: 0; right: 0;
    display: flex;
    justify-content: center;
    align-items: center;
    gap: 1.5rem;
  }
  button {
    width: 7rem;
    height: 2.2rem;
  }
</style>
