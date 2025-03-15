<script>
    let formData = {
        email: '',
        password: '',
        confirmPassword: '',
    };
    let errors = {};
    
    function validateForm() {
        errors = {};
        if (!formData.email) errors.email = 'Email is required';
        else if (!/^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(formData.email)) errors.email = 'Invalid email format';
        
        if (!formData.password) errors.password = 'Password is required';
        else if (formData.password.length < 8) errors.password = 'Password must be at least 8 characters';
        
        if (!formData.confirmPassword) errors.confirmPassword = 'Please confirm your password';
        else if (formData.password !== formData.confirmPassword) errors.confirmPassword = 'Passwords do not match';

        return Object.keys(errors).length === 0;
    }

    async function handleSubmit() {
        if (validateForm()) {
            try {
                // TODO: Implement API call to register user
                console.log('Form submitted:', formData);
            } catch (error) {
                console.error('Registration error:', error);
            }
        }
    }

    async function handleOAuthSignup(provider) {
        try {
            // TODO: Implement OAuth signup
            console.log(`Signing up with ${provider}`);
        } catch (error) {
            console.error(`${provider} signup error:`, error);
        }
    }
</script>

<main class="min-h-screen bg-gray-900 flex items-center justify-center px-4 py-12">
    <div class="bg-gray-800 p-8 rounded-xl shadow-lg w-1/2 max-w-md border border-gray-700">
        <h1 class="text-3xl font-bold text-center mb-8 text-white">Create Account</h1>
        
        <form on:submit|preventDefault={handleSubmit} class="space-y-6" >
            <div>
                <label for="email" class="block text-sm font-medium text-gray-300 mb-1">Email</label>
                <input
                    type="email"
                    id="email"
                    bind:value={formData.email}
                    class="w-full px-4 py-2 rounded-lg bg-gray-700 border border-gray-600 focus:outline-none focus:border-cyan-400"
                    placeholder="email@example.com"
                />
                {#if errors.email}
                    <p class="text-red-400 text-sm mt-1">{errors.email}</p>
                {/if}
            </div>

            <div>
                <label for="password" class="block text-sm font-medium text-gray-300 mb-1">Password</label>
                <input
                    type="password"
                    id="password"
                    bind:value={formData.password}
                    class="w-full px-4 py-2 rounded-lg bg-gray-700 border border-gray-600 focus:outline-none focus:border-cyan-400"
                    placeholder="••••••••"
                />
                {#if errors.password}
                    <p class="text-red-400 text-sm mt-1">{errors.password}</p>
                {/if}
            </div>

            <div>
                <label for="confirmPassword" class="block text-sm font-medium text-gray-300 mb-1">Confirm Password</label>
                <input
                    type="password"
                    id="confirmPassword"
                    bind:value={formData.confirmPassword}
                    class="w-full px-4 py-2 rounded-lg bg-gray-700 border border-gray-600 focus:outline-none focus:border-cyan-400"
                    placeholder="••••••••"
                />
                {#if errors.confirmPassword}
                    <p class="text-red-400 text-sm mt-1">{errors.confirmPassword}</p>
                {/if}
            </div>
            <br>

            <button
                type="submit"
                class="w-full bg-gradient-to-r from-blue-500 to-cyan-500 hover:from-blue-600 hover:to-cyan-600 
                text-white font-bold py-3 px-4 rounded-lg text-lg shadow-lg transform transition-all duration-300 hover:scale-105"
            >
                Sign Up
            </button>
        </form>

        <div class="mt-6">
            <div class="relative">
                <div class="absolute inset-0 flex items-center">
                    <div class="w-full border-t border-gray-600"></div>
                </div>
                <div class="relative flex justify-center text-sm">
                    <span class="px-2 bg-gray-800 text-gray-400">Or continue with</span>
                </div>
            </div>

            <div class="mt-6 grid grid-cols-2 gap-3">
                <button
                    on:click={() => handleOAuthSignup('google')}
                    class="flex w-full items-center justify-center gap-3 rounded-lg bg-gray-700 px-4 py-3 text-white hover:bg-gray-600 focus:outline-none focus:ring-2 focus:ring-cyan-400 focus:ring-offset-2 focus:ring-offset-gray-800"
                >
                    <svg class="h-5 w-5" viewBox="0 0 24 24">
                        <path
                            fill="currentColor"
                            d="M22.56 12.25c0-.78-.07-1.53-.2-2.25H12v4.26h5.92c-.26 1.37-1.04 2.53-2.21 3.31v2.77h3.57c2.08-1.92 3.28-4.74 3.28-8.09z"
                        />
                        <path
                            fill="currentColor"
                            d="M12 23c2.97 0 5.46-.98 7.28-2.66l-3.57-2.77c-.98.66-2.23 1.06-3.71 1.06-2.86 0-5.29-1.93-6.16-4.53H2.18v2.84C3.99 20.53 7.7 23 12 23z"
                        />
                        <path
                            fill="currentColor"
                            d="M5.84 14.09c-.22-.66-.35-1.36-.35-2.09s.13-1.43.35-2.09V7.07H2.18C1.43 8.55 1 10.22 1 12s.43 3.45 1.18 4.93l2.85-2.22.81-.62z"
                        />
                        <path
                            fill="currentColor"
                            d="M12 5.38c1.62 0 3.06.56 4.21 1.64l3.15-3.15C17.45 2.09 14.97 1 12 1 7.7 1 3.99 3.47 2.18 7.07l3.66 2.84c.87-2.6 3.3-4.53 6.16-4.53z"
                        />
                    </svg>
                    <span class="text-sm font-semibold">Google</span>
                </button>

                <button
                    on:click={() => handleOAuthSignup('github')}
                    class="flex w-full items-center justify-center gap-3 rounded-lg bg-gray-700 px-4 py-3 text-white hover:bg-gray-600 focus:outline-none focus:ring-2 focus:ring-cyan-400 focus:ring-offset-2 focus:ring-offset-gray-800"
                >
                    <svg class="h-5 w-5" fill="currentColor" viewBox="0 0 24 24">
                        <path
                            fill-rule="evenodd"
                            clip-rule="evenodd"
                            d="M12 2C6.477 2 2 6.463 2 11.97c0 4.404 2.865 8.14 6.839 9.458.5.092.682-.216.682-.48 0-.236-.008-.864-.013-1.695-2.782.602-3.369-1.337-3.369-1.337-.454-1.151-1.11-1.458-1.11-1.458-.908-.618.069-.606.069-.606 1.003.07 1.531 1.027 1.531 1.027.892 1.524 2.341 1.084 2.91.828.092-.643.35-1.083.636-1.332-2.22-.251-4.555-1.107-4.555-4.927 0-1.088.39-1.979 1.029-2.675-.103-.252-.446-1.266.098-2.638 0 0 .84-.268 2.75 1.022A9.606 9.606 0 0112 6.82c.85.004 1.705.114 2.504.336 1.909-1.29 2.747-1.022 2.747-1.022.546 1.372.202 2.386.1 2.638.64.696 1.028 1.587 1.028 2.675 0 3.83-2.339 4.673-4.566 4.92.359.307.678.915.678 1.846 0 1.332-.012 2.407-.012 2.734 0 .267.18.577.688.48C19.137 20.107 22 16.373 22 11.969 22 6.463 17.522 2 12 2z"
                        />
                    </svg>
                    <span class="text-sm font-semibold">GitHub</span>
                </button>
            </div>
        </div>

        <p class="text-center mt-6 text-gray-400">
            Already have an account?
            <a href="/login" class="text-cyan-400 hover:text-cyan-300">Log in</a>
        </p>
    </div>
</main>
