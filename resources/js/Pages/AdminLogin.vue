<template>
    <div class="container col-4 mt-5">
        <form @submit.prevent="login">
            <!-- Email input -->
            <div class="form-outline mb-4">
                <input type="email" id="form2Example1" class="form-control" v-model="email" @input="checkEmail"  />
                <label class="form-label" for="form2Example1">Email address</label>
                 <br>
                <small v-if="showEmailError && emailError" class="text-danger">
                    {{ emailError }}
                </small>
            </div>

            <!-- Password input -->
            <div class="form-outline mb-4">
                <input type="password" id="form2Example2" class="form-control" v-model="password" @input="checkPassword"  />
                <label class="form-label" for="form2Example2">Password</label>
                <br>
                <small v-if="showPasswordError && passwordError" class="text-danger" style="margin-top:-50px" >
                    {{ passwordError }}
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
import { Inertia } from '@inertiajs/inertia';

export default {
    data() {
        return {
            email: '',
            password: '',
            emailError: '',
            passwordError: '',
            showEmailError: false,
            showPasswordError: false,
            loginAttempted: false,
        };
    },
    methods: {
        async login() {
            this.loginAttempted = true;
            this.checkEmail();
            this.checkPassword();
            if (this.emailError || this.passwordError) {
                return; // Do not submit the form if there are validation errors
            }

            try {
                const formData = {
                    email: this.email,
                    password: this.password,
                };
                const response = await axios.post('/admin_login', formData);
                if (response.data.status) {
                    Inertia.visit('/admin_dashboard');
                }
            } catch (error) {
                console.error(error);
            }
        },
        checkEmail() {
            if (!this.email && this.loginAttempted) {
                this.emailError = 'Email is required.';
                this.showEmailError = true;
            } else {
                this.emailError = '';
                this.showEmailError = false;
            }
        },
        checkPassword() {
            if (this.password.length >= 8) {
                const regex = /^(?=.*[A-Z])(?=.*[a-z])(?=.*[!@#$%^&*])(?=.*[0-9])[A-Za-z\d!@#$%^&*]{8,}$/;
                this.showPasswordError = this.loginAttempted && !regex.test(this.password);
                this.passwordError = !regex.test(this.password)
                    ? 'Password must have at least one capital letter, one small letter, one special character, and one number.'
                    : '';
            } else if (this.loginAttempted) {
                this.passwordError = 'Password is required.';
                this.showPasswordError = true;
            } else {
                this.passwordError = '';
                this.showPasswordError = false;
            }
        }
    }
};
</script>
