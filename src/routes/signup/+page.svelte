<script>
    let formData = {
        email: '',
        password: '',
        confirmPassword: '',
        name: ''
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
        
        if (!formData.name) errors.name = 'Name is required';
        
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
</script>

<main class="min-h-screen bg-gray-900 flex items-center justify-center px-4 py-12">
    <div class="bg-gray-800 p-8 rounded-xl shadow-lg w-1/2 max-w-md border border-gray-700">
        <h1 class="text-3xl font-bold text-center mb-8 text-white">Create Account</h1>
        
        <form on:submit|preventDefault={handleSubmit} class="space-y-6" >
            <div>
                <label for="name" class="block text-sm font-medium text-gray-300 mb-1">Full Name</label>
                <input
                    type="text"
                    id="name"
                    bind:value={formData.name}
                    class="w-full px-4 py-2 rounded-lg bg-gray-700 border border-gray-600 focus:outline-none focus:border-cyan-400"
                    placeholder="Username"
                />
                {#if errors.name}
                    <p class="text-red-400 text-sm mt-1">{errors.name}</p>
                {/if}
            </div>

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

        <p class="text-center mt-6 text-gray-400">
            Already have an account?
            <a href="/login" class="text-cyan-400 hover:text-cyan-300">Log in</a>
        </p>
    </div>
</main>
