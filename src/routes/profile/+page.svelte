<script>
    let profile = {
        email: 'user@example.com', // TODO: Get from auth state
        fullName: '',
        title: '',
        bio: '',
        skills: [],
        location: '',
        githubUrl: '',
        linkedinUrl: '',
        portfolioUrl: ''
    };

    let newSkill = '';
    let resumeFile = null;
    let profileImage = null;
    let isEditing = true;
    let dragActiveResume = false;
    let dragActiveProfile = false;

    function addSkill() {
        if (newSkill.trim() && !profile.skills.includes(newSkill.trim())) {
            profile.skills = [...profile.skills, newSkill.trim()];
            newSkill = '';
        }
    }

    function removeSkill(skill) {
        profile.skills = profile.skills.filter(s => s !== skill);
    }

    function handleDragEnter(event, type) {
        event.preventDefault();
        if (type === 'resume') dragActiveResume = true;
        if (type === 'profile') dragActiveProfile = true;
    }

    function handleDragLeave(event, type) {
        event.preventDefault();
        if (type === 'resume') dragActiveResume = false;
        if (type === 'profile') dragActiveProfile = false;
    }

    function handleDrop(event, type) {
        event.preventDefault();
        if (type === 'resume') dragActiveResume = false;
        if (type === 'profile') dragActiveProfile = false;

        const file = event.dataTransfer?.files[0];
        if (file) {
            if (type === 'resume' && (file.type === 'application/pdf' || file.type.includes('word'))) {
                handleResumeUpload({ target: { files: [file] } });
            } else if (type === 'profile' && file.type.startsWith('image/')) {
                handleProfileImageUpload({ target: { files: [file] } });
            }
        }
    }

    async function handleResumeUpload(event) {
        const file = event.target.files[0];
        if (file && (file.type === 'application/pdf' || file.type.includes('word'))) {
            // TODO: Implement file upload to server
            resumeFile = file;
            console.log('Resume uploaded:', file.name);
        }
    }

    async function handleProfileImageUpload(event) {
        const file = event.target.files[0];
        if (file && file.type.startsWith('image/')) {
            // TODO: Implement image upload to server
            profileImage = file;
            console.log('Profile image uploaded:', file.name);
        }
    }

    async function saveProfile() {
        try {
            // TODO: Implement profile update API call
            console.log('Profile saved:', profile);
            isEditing = false;
        } catch (error) {
            console.error('Error saving profile:', error);
        }
    }
</script>

<main class="min-h-screen bg-gray-900 py-12 px-4 sm:px-6 lg:px-8">
    <div class="max-w-2xl mx-auto w-1/2"> 
        <div class="bg-gray-800 shadow-xl rounded-lg overflow-hidden">
            <!-- Profile Header -->
            <div class="relative h-48 bg-gradient-to-r from-blue-500 to-cyan-500">
                <!-- Upload Card for Profile Image -->
                <div class="absolute -bottom-16 left-8">
                    <div class="relative">
                        <div
                            class="w-32 h-32 rounded-full border-4 border-gray-800 bg-gray-700 overflow-hidden cursor-pointer transition-all duration-200"
                            on:dragenter={(e) => handleDragEnter(e, 'profile')}
                            on:dragleave={(e) => handleDragLeave(e, 'profile')}
                            on:dragover|preventDefault
                            on:drop|preventDefault={(e) => handleDrop(e, 'profile')}
                            class:ring-2={dragActiveProfile}
                            class:ring-cyan-400={dragActiveProfile}
                            class:scale-105={dragActiveProfile}
                        >
                            {#if profileImage}
                                <img src={URL.createObjectURL(profileImage)} alt="Profile" class="w-full h-full object-cover" />
                            {:else}
                                <div class="w-full h-full flex items-center justify-center text-gray-400">
                                    <svg class="w-16 h-16" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                                        <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M16 7a4 4 0 11-8 0 4 4 0 018 0zM12 14a7 7 0 00-7 7h14a7 7 0 00-7-7z" />
                                    </svg>
                                </div>
                            {/if}
                            <div class="absolute inset-0 flex items-center justify-center bg-black bg-opacity-50 opacity-0 hover:opacity-100 transition-opacity">
                                <label class="cursor-pointer w-full h-full flex flex-col items-center justify-center">
                                    <input type="file" class="hidden" accept="image/*" on:change={handleProfileImageUpload} />
                                    <div class="text-white text-center p-2">
                                        <span class="text-sm font-medium" style="-webkit-clip-path: url(#circle);">{dragActiveProfile ? 'Drop to Upload' : 'Upload Photo'}</span>
                                        <svg class="w-8 h-8 mx-auto mb-2" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                                            <clipPath id="circle">
                                                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 16l4.586-4.586a2 2 0 012.828 0L16 16m-2-2l1.586-1.586a2 2 0 012.828 0L20 14m-6-6h.01M6 20h12a2 2 0 002-2V6a2 2 0 00-2-2H6a2 2 0 00-2 2v12a2 2 0 002 2z" />
                                            </clipPath>
                                        </svg>
                                    </div>
                                </label>
                            </div>
                        </div>
                    </div>
                </div>
                <div class="absolute top-4 right-4">
                    <button
                        on:click={() => isEditing = !isEditing}
                        class="px-4 py-2 rounded-lg bg-gray-800 text-white hover:bg-gray-700 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-offset-gray-800 focus:ring-cyan-400"
                    >
                        {isEditing ? 'Cancel' : 'Edit Profile'}
                    </button>
                </div>
            </div>

            <!-- Profile Content -->
            <div class="pt-20 px-8 pb-8">
                <div class="space-y-6">
                    <!-- Basic Info -->
                    <div class="grid grid-cols-1 gap-6 sm:grid-cols-2">
                        <div>
                            <label class="block text-sm font-medium text-gray-300">Full Name</label>
                            <input
                                type="text"
                                bind:value={profile.fullName}
                                disabled={!isEditing}
                                class="mt-1 w-full px-4 py-2 rounded-lg bg-gray-700 border border-gray-600 text-white focus:outline-none focus:border-cyan-400 disabled:opacity-50"
                                placeholder="Your full name"
                            />
                        </div>
                        <div>
                            <label class="block text-sm font-medium text-gray-300">Title</label>
                            <input
                                type="text"
                                bind:value={profile.title}
                                disabled={!isEditing}
                                class="mt-1 w-full px-4 py-2 rounded-lg bg-gray-700 border border-gray-600 text-white focus:outline-none focus:border-cyan-400 disabled:opacity-50"
                                placeholder="e.g. Full Stack Developer"
                            />
                        </div>
                    </div>

                    <!-- Bio -->
                    <div>
                        <label class="block text-sm font-medium text-gray-300">Bio</label>
                        <textarea
                            bind:value={profile.bio}
                            disabled={!isEditing}
                            rows="4"
                            class="mt-1 w-full px-4 py-2 rounded-lg bg-gray-700 border border-gray-600 text-white focus:outline-none focus:border-cyan-400 disabled:opacity-50"
                            placeholder="Tell us about yourself"
                        ></textarea>
                    </div>

                    <!-- Skills -->
                    <div>
                        <label class="block text-sm font-medium text-gray-300 mb-2">Skills</label>
                        <div class="flex flex-wrap gap-2">
                            {#each profile.skills as skill}
                                <span class="px-3 py-1 rounded-full bg-gray-700 text-white flex items-center">
                                    {skill}
                                    {#if isEditing}
                                        <button
                                            on:click={() => removeSkill(skill)}
                                            class="ml-2 text-gray-400 hover:text-red-400"
                                        >
                                            ×
                                        </button>
                                    {/if}
                                </span>
                            {/each}
                            {#if isEditing}
                                <div class="flex items-center">
                                    <input
                                        type="text"
                                        bind:value={newSkill}
                                        placeholder="Add skill"
                                        class="px-3 py-1 rounded-lg bg-gray-700 border border-gray-600 text-white focus:outline-none focus:border-cyan-400"
                                        on:keydown={(e) => e.key === 'Enter' && addSkill()}
                                    />
                                    <button
                                        on:click={addSkill}
                                        class="ml-2 text-cyan-400 hover:text-cyan-300"
                                    >
                                        Add
                                    </button>
                                </div>
                            {/if}
                        </div>
                    </div>

                    <!-- Social Links -->
                    <div class="grid grid-cols-1 gap-6 sm:grid-cols-2">
                        <div>
                            <label class="block text-sm font-medium text-gray-300">GitHub URL</label>
                            <input
                                type="url"
                                bind:value={profile.githubUrl}
                                disabled={!isEditing}
                                class="mt-1 w-full px-4 py-2 rounded-lg bg-gray-700 border border-gray-600 text-white focus:outline-none focus:border-cyan-400 disabled:opacity-50"
                                placeholder="https://github.com/username"
                            />
                        </div>
                        <div>
                            <label class="block text-sm font-medium text-gray-300">LinkedIn URL</label>
                            <input
                                type="url"
                                bind:value={profile.linkedinUrl}
                                disabled={!isEditing}
                                class="mt-1 w-full px-4 py-2 rounded-lg bg-gray-700 border border-gray-600 text-white focus:outline-none focus:border-cyan-400 disabled:opacity-50"
                                placeholder="https://linkedin.com/in/username"
                            />
                        </div>
                    </div>

                    <!-- Resume Upload Card -->
                    <div>
                        <label class="block text-sm font-medium text-gray-300 mb-2">Resume</label>
                        <div
                            class="border-2 border-dashed border-gray-600 rounded-lg p-6 text-center cursor-pointer transition-all duration-200"
                            on:dragenter={(e) => handleDragEnter(e, 'resume')}
                            on:dragleave={(e) => handleDragLeave(e, 'resume')}
                            on:dragover|preventDefault
                            on:drop|preventDefault={(e) => handleDrop(e, 'resume')}
                            class:border-cyan-400={dragActiveResume}
                            class:bg-gray-700={dragActiveResume}
                            class:scale-[1.02]={dragActiveResume}
                        >
                            <label class="cursor-pointer block w-full h-full">
                                <input type="file" class="hidden" accept=".pdf,.doc,.docx" on:change={handleResumeUpload} />
                                <div class="space-y-2">
                                    <svg class="w-12 h-12 mx-auto text-gray-400" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                                        <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 16v1a3 3 0 003 3h10a3 3 0 003-3v-1m-4-8l-4-4m0 0L8 8m4-4v12" />
                                    </svg>
                                    {#if resumeFile}
                                        <p class="text-gray-300">{resumeFile.name}</p>
                                        <p class="text-sm text-gray-400">{dragActiveResume ? 'Drop to Replace' : 'Click or drag to replace'}</p>
                                    {:else}
                                        <p class="text-gray-300">{dragActiveResume ? 'Drop your file here' : 'Drop your resume here or click to browse'}</p>
                                        <p class="text-sm text-gray-400">Supports PDF, DOC, DOCX</p>
                                    {/if}
                                </div>
                            </label>
                        </div>
                    </div>

                    {#if isEditing}
                        <div class="flex justify-end pt-4">
                            <button
                                on:click={saveProfile}
                                class="px-6 py-2 rounded-lg bg-gradient-to-r from-blue-500 to-cyan-500 text-white font-semibold hover:from-blue-600 hover:to-cyan-600 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-offset-gray-800 focus:ring-cyan-400"
                            >
                                Save Changes
                            </button>
                        </div>
                    {/if}
                </div>
            </div>
        </div>
    </div>
</main>
