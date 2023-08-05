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
  let username = "";
  let password = "";
  let noticeText = "";
</script>

<div class="login-page">
  <h4 class="notice warning">{noticeText}</h4>
  <h1>Please Log In</h1>

  <div class="fields">
    <div class="field">
      Username:
      <input bind:value={username} placeholder="Username">
    </div>
    <div class="field">
      Password:
      <input bind:value={password} placeholder="Password">
    </div>
    <div class="button-wrapper">
      <button>Log In</button>
    </div>
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
  button {
    width: 6rem;
    height: 2rem;
  }
</style>
