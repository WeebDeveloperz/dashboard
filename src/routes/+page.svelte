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
  import { BASE_URL } from "../config.js"
  let username = "";
  let password = "";
  let noticeText = "";

  const handleLogin = _ => {
    noticeText = username == "" ? "Username Can't Be Blank." : ""
    noticeText = noticeText == "" ? (password == "" ? "Password Can't Be Blank." : "") : noticeText;
    if (noticeText != "") return;

    const data = new FormData();
    data.append("username", username);
    data.append("password", password)

    const res = fetch(BASE_URL + "auth/login", {
      method: "POST",
      body: data
    })
      .then(response => response.json())
      .then(data => {
        console.log(data)
        if (data.error) {
          noticeText = `Error: ${data.error}`;
        } else {
          sessionStorage.setItem("authtoken", data.token);
          window.location.replace("/home");
        }
      });
  }
</script>

<div class="login-page">
  <h1>Please Log In</h1>

  <div class="fields">
    <div class="field">
      Username:
      <input bind:value={username} placeholder="Username">
    </div>
    <div class="field">
      Password:
      <input type="password" bind:value={password} placeholder="Password">
    </div>
    <div class="button-wrapper">
      <button on:click={handleLogin}>Log In</button>
    </div>
    <h4 class="notice warning">{noticeText}</h4>
  </div>
</div>

<style>
  .login-page {
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    height: 90vh;
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
</style>
