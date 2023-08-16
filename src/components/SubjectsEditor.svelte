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

  export let subject = null;
  let fObject = null;

  let noticeText = ""
  const validate = (subject) => {
    if (fObject == null)
      return "No file uploaded";
    return "";
  }

  const handleSave = async(method) => {
    noticeText = validate(subject);
    if (noticeText != "" && method !== "delete") return;
    noticeText = method === "put" ? "File updation is disabled. Please delete file and upload again." : "";
    if (noticeText != "") return;

    subject.sem = parseInt(subject.sem)
    const data = new FormData();
    data.append("data", JSON.stringify(subject));
    data.append("token", sessionStorage.getItem("authtoken"));

    const res = await fetch(BASE_URL + "subjects", {
      method: method,
      body: data
    })
      .then(response => response.json())
      .then(data => {
        if (data.error) {
          alert(data.error)
        } else {
          dispatch("subjects-updated", {});
        }
      });
  }

  const handleCancel = () =>
    dispatch("edit-canceled", {})
</script>


<div class="subject-editor">
  <h4 class="notice warning">{noticeText}</h4>
  <div class="fields">
    <div class="field">
      Branch:
      <input bind:value={subject.branch} placeholder="Branch">
    </div>
    <div class="field">
      Semester:
      <input bind:value={subject.sem} placeholder="Semester" type="number">
    </div>
    <div class="field">
      Subject Name:
      <input bind:value={subject.name} placeholder="Subject Name">
    </div>
    <div class="field">
      Subject Code:
      <input bind:value={subject.code} placeholder="Subject Code">
    </div>
    <div class="field">
      Upload Card Image:
      <input type="file" accept=".webp" on:change={e => fObject = e.target.files[0]}>
    </div>
  </div>

  <div class="options-wrapper">
    <div class="options">
      {#if subject.id != null}
        <button on:click={() => handleSave("delete")}>Delete</button>
      {/if}
    </div>
    <div class="options">
      <button on:click={handleCancel}>Cancel</button>
      <button on:click={() => handleSave(subject.id == null ? "post" : "put")}>Save</button>
    </div>
  </div>
</div>

<style>
  .subject-editor {
    color: white;
    position: relative;
    height: 100%;
    width: 100%;
    display: flex;
    justify-content: space-around;
    align-items: center;
  }
  input {
    background-color: #212121;
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
  .options-alt {
    position: absolute;
    bottom: 3rem;
    left: 4rem;
    display: flex;
    width: 12rem;
    justify-content: space-between;
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
  .field input {
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
