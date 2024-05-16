<template>
    <div class="container col-4 mt-5">
        <form @submit.prevent="login">
            <!-- Email input -->
            <div class="form-outline mb-4">
                <input type="email" id="form2Example1" class="form-control" v-model="email" required />
                <label class="form-label" for="form2Example1">Email address</label>
            </div>

            <!-- Password input -->
            <div class="form-outline mb-4">
                <input type="password" id="form2Example2" class="form-control" v-model="password" @input="checkPassword" required />
                <label class="form-label" for="form2Example2">Password</label>
                <br>
                <small v-if="password.length > 0 && !isValidPassword" class="text-danger">
                    Password must have at least one capital letter , small letter and one special character.
                </small>
            </div>

            <!-- 2 column grid layout for inline styling -->
            <div class="row mb-4">
                <div class="col d-flex justify-content-center">
                    <!-- Checkbox -->
                    <div class="form-check">
                        <input class="form-check-input" type="checkbox" value="" id="form2Example31" checked />
                        <label class="form-check-label" for="form2Example31"> Remember me </label>
                    </div>
                </div>

                <div class="col">
                    <!-- Simple link -->
                    <a href="#!">Forgot password?</a>
                </div>
            </div>

            <!-- Submit button -->
            <button type="submit" class="btn btn-primary btn-block mb-4">Sign in</button>

            <!-- Register buttons -->
            <div class="text-center">
                <p>Not a member? <a href="#!">Register</a></p>
                <p>or sign up with:</p>
                <button type="button" class="btn btn-link btn-floating mx-1">
                    <i class="fab fa-facebook-f"></i>
                </button>

                <button type="button" class="btn btn-link btn-floating mx-1">
                    <i class="fab fa-google"></i>
                </button>

                <button type="button" class="btn btn-link btn-floating mx-1">
                    <i class="fab fa-twitter"></i>
                </button>

                <button type="button" class="btn btn-link btn-floating mx-1">
                    <i class="fab fa-github"></i>
                </button>
            </div>
        </form>
    </div>
</template>

<script>
import axios from "axios";

export default {
    data() {
        return {
            email: '',
            password: '',
            isValidPassword: true,
        };
    },
    methods: {
        async login() {
            if (!this.isValidPassword) {
                return; // Do not submit form if password is not valid
            }

            try {
                const formData = {
                    email: this.email,
                    password: this.password,
                };
                const response = await axios.post('/admin_login', formData);
                console.log(response);
                // Handle the response as needed, such as redirecting or showing a success message
            } catch (error) {
                console.error(error);
                // Handle error response, e.g., display error message to the user
            }
        },
        checkPassword() {
            if (this.password.length >= 8) {
                const regex = /^(?=.*[A-Z])(?=.*[!@#$%^&*])[A-Za-z\d!@#$%^&*]{8,}$/;
                this.isValidPassword = regex.test(this.password);
            } else {
                this.isValidPassword = true; // Reset validation when password length is less than 8
            }
        }
    }
};
</script>
