<template>
    <div class="form">
    <div class="Image">
    <img src="../public/icons/login.svg" alt="" class="login-icon" />
</div>
<div class="login-form">
    <p>Type an email and password to login to MERC-Portal</p>
    <div class="text-input">Email:
    <input type="text" v-model="username" placeholder="Enter your email" />
    </div>
    <div class="text-input">Password:
    <input type="password" v-model="password" placeholder="Enter your password" />
    </div>
</div>
<div class="buttons">
        <button @click="login">Login</button>
        <button @click="register">Register</button>
        <button @click="forgotPassword">Forgot Password</button>
    </div>
    </div>

</template>

<script
setup></script>
<style>
.form {
    display: flex;
    flex-direction: row;
    align-items: center;
    justify-content: center;
    height: 100vh;
    width: 100%;
    background-color: #c3c7cb;
    justify-content: space-around;
}
.text-input {
    margin: 5px 0;
}
.login-icon {
    width: 100px;
    height: 100px;
}
.buttons {
    display: flex;
    flex-direction: column;
}
.login-form {
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
}
</style>