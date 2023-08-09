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
  import { BASE_URL } from "../../config.js"
  import Fa from 'svelte-fa/src/fa.svelte'
  import { faArrowLeft, faRotateRight } from '@fortawesome/free-solid-svg-icons'

  let fObject = null;

  let noticeText = ""
  const validate = () => {
    if (fObject == null)
      return "No file uploaded";
    return "";
  }

  const handleSave = async(method) => {
    noticeText = validate();
    if (noticeText != "") return;

    const data = new FormData();
    data.append("file", fObject)
    data.append("token", sessionStorage.getItem("authtoken"));

    const res = await fetch(BASE_URL + "other/set-index-post", {
      method: "post",
      body: data
    })
      .then(response => response.json())
      .then(r => {
        if (r.error) {
          alert(r.error)
        } else {
          handleCancel()
        }
      });
  }

  const handleCancel = _ => window.location.replace("/home");
</script>

<div class="back-button">
  <a href="/home">
    <Fa icon={faArrowLeft} />
  </a>
</div>

<div class="img-editor">
  <h4 class="notice warning">{noticeText}</h4>
  <div class="fields">
    <div class="field">
      Upload File:
      <input type="file" accept=".webp" on:change={e => fObject = e.target.files[0]}>
    </div>
  </div>
  <div class="options-wrapper">
    <div class="options">
      <button on:click={handleCancel}>Cancel</button>
      <button on:click={handleSave}>Save</button>
    </div>
  </div>
</div>

<style>
  * {
    font-family: sans-serif;
  }
  h1 {
    color: white;
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
  button {
    padding: 0.4rem 0.2rem;
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
  .back-button {
    font-size: 2rem;
    position: fixed;
    top: 1rem;
  }
  .back-button a {
    color: white;
    text-decoration: none;
  }
  .back-button a:hover {
    color: #b481da;
  }
  .back-button { left: 1rem; }
  .warning {
    color: red;
  }
  .notice {
    position: absolute;
    top: 2rem;
    left: 0; right: 0;
    margin: auto;
    text-align: center;
  }
  .img-editor {
    color: white;
    height: 95vh;
    width: 100%;
    display: flex;
    flex-direction: column;
    justify-content: center;
    gap: 2rem;
    align-items: center;
  }
  .options-wrapper {
    width: 100%;
    display: flex;
    justify-content: center;
    gap: 2rem;
  }
  .options {
    display: flex;
    justify-content: center;
    gap: 2rem;
  }
</style>
