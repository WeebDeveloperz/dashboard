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
  import { createEventDispatcher } from 'svelte';
  import { BASE_URL } from "../config.js"

  const dispatch = createEventDispatcher();

  export let file = null;
  let fObject = null;

  let noticeText = ""
  const validate = (file) => {
    if (file.name == "")
      return "Please enter a file name";
    if (fObject == null)
      return "No file uploaded";
    return "";
  }

  let subjects = [];
  const loadSubjects = () =>
    fetch(BASE_URL + 'subjects')
      .then(response => response.json())
      .then(data => subjects = data.data);
  loadSubjects();

  const handleSave = async(method) => {
    noticeText = validate(file);
    if (noticeText != "" && method !== "delete") return;
    noticeText = method === "put" ? "File updation is disabled. Please delete file and upload again." : "";
    if (noticeText != "") return;

    const data = new FormData();
    data.append("data", JSON.stringify(file));
    data.append("file", fObject)
    data.append("token", sessionStorage.getItem("authtoken"));

    const res = await fetch(BASE_URL + "files", {
      method: method,
      body: data
    })
      .then(response => response.json())
      .then(r => {
        if (r.error) {
          alert(r.error)
        } else {
          dispatch("files-updated", {});
        }
      });
  }

  const handleCancel = () =>
    dispatch("edit-canceled", {})
</script>


<div class="file-editor">
  <h4 class="notice warning">{noticeText}</h4>
  <div class="fields">
    <div class="field">
      File Name:
      <input bind:value={file.name} placeholder="File Name">
    </div>
    <div class="field">
      Subject:
      <select bind:value={file.subject}>
        <option value="" disabled selected>
          Please Select
        </option>
        {#each subjects as sub}
          <option value={sub}>
            {`${sub.name} (${sub.branch} sem ${sub.sem})`}
          </option>
        {/each}
      </select>
    </div>
    <div class="field">
      Upload File:
      <input type="file" accept=".pdf" on:change={e => fObject = e.target.files[0]}>
    </div>
  </div>
  <div class="options-wrapper">
    <div class="options">
      {#if file.id != null}
        <button on:click={() => handleSave("delete")}>Delete</button>
      {/if}
    </div>
    <div class="options">
      <button on:click={handleCancel}>Cancel</button>
      <button on:click={e => handleSave(file.id == null ? "post" : "put")}>Save</button>
    </div>
  </div>
</div>

<style>
  .file-editor {
    color: white;
    position: relative;
    height: 100%;
    width: 100%;
    display: flex;
    justify-content: space-around;
    align-items: center;
  }
  input, select {
    background-color: #212121;
    border: 1px solid #dfdfdf;
    padding: 0.3rem;
    font-size: 0.9em;
    outline: 0;
    border-radius: 4px;
    color: white;
    box-sizing: border-box;
  }
  input[type=file] {
    border: none;
    background-color: #232627;
  }
  input:focus, select:focus {
    border: 1px solid #b481da;
  }
  input::placeholder {
    color: gray;
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
  .options-wrapper {
    gap: 2rem;
    flex-wrap: wrap;
    width: calc(100% - 6rem);
    margin: auto;
    position: absolute;
    bottom: 3rem;
    display: flex;
    justify-content: space-between;
  }
  .options {
    display: flex;
    gap: 1.5rem;
  }
  .field {
    width: 30rem;
    display: flex;
    justify-content: space-between;
    margin: 0.5rem 0;
  }
  .field input, .field select {
    width: 60%;
  }
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
</style>
