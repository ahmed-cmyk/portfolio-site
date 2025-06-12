<script>
  import { onMount } from 'svelte';
  import { fly, fade } from 'svelte/transition';

  // State for the mobile menu
  let mobileMenuOpen = false;

  // --- Reactive variables to control scroll-triggered animations ---
  // These flags will be set to true by the IntersectionObserver when a section is visible.
  let heroContentLoaded = false;
  let aboutVisible = false;
  // let projectsVisible = false; // Commented out as per request
  let servicesVisible = false;
  let approachVisible = false;
  let skillsVisible = false;
  let contactVisible = false;

  // onMount runs after the component is first rendered to the DOM.
  onMount(() => {
    // We set a small timeout to ensure the DOM is fully settled before we start animations.
    const heroTimer = setTimeout(() => (heroContentLoaded = true), 100);

    // --- Intersection Observer Setup ---
    const options = {
      root: null, // observes intersections relative to the viewport
      threshold: 0.15 // trigger when 15% of the element is visible
    };

    const observer = new IntersectionObserver((entries) => {
      entries.forEach(entry => {
        if (entry.isIntersecting) {
          // Set the corresponding reactive variable to true when the section becomes visible.
          switch (entry.target.id) {
            case 'about':
              aboutVisible = true;
              break;
            // case 'projects': // Commented out
            //   projectsVisible = true;
            //   break;
            case 'services':
              servicesVisible = true;
              break;
            case 'approach':
              approachVisible = true;
              break;
            case 'skills':
              skillsVisible = true;
              break;
            case 'contact':
              contactVisible = true;
              break;
          }
          // Once a section is visible, we don't need to observe it anymore.
          observer.unobserve(entry.target);
        }
      });
    }, options);

    // Observe each section by its ID.
    const sections = document.querySelectorAll('#about, #services, #approach, #skills, #contact');
    sections.forEach(section => observer.observe(section));

    // Cleanup when the component is destroyed to prevent memory leaks.
    return () => {
      clearTimeout(heroTimer);
      sections.forEach(section => observer.unobserve(section));
    };
  });
</script>

<div class="antialiased">
  <!-- Header -->
  <header class="fixed top-0 left-0 right-0 z-50 bg-black/30 backdrop-blur-md">
    <div class="container mx-auto px-6 py-4 flex justify-between items-center">
      <a href="#hero" class="text-2xl font-bold gradient-text">Ahmed.</a>
      <nav class="hidden md:flex space-x-8">
        <a href="#about" class="hover:text-sky-400 transition-colors">About</a>
        <a href="#services" class="hover:text-sky-400 transition-colors">Services</a>
        <a href="#approach" class="hover:text-sky-400 transition-colors">Approach</a>
        <a href="#skills" class="hover:text-sky-400 transition-colors">Skills</a>
        <a href="#contact" class="hover:text-sky-400 transition-colors">Contact</a>
      </nav>
      <a href="https://github.com/ahmed-cmyk" target="_blank" rel="noopener noreferrer" class="hidden md:block bg-slate-800 hover:bg-slate-700 text-white font-semibold py-2 px-4 rounded-lg transition-colors">
        GitHub
      </a>
      <button on:click={() => mobileMenuOpen = !mobileMenuOpen} class="md:hidden text-2xl">
        <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><line x1="3" y1="12" x2="21" y2="12"></line><line x1="3" y1="6" x2="21" y2="6"></line><line x1="3" y1="18" x2="21" y2="18"></line></svg>
      </button>
    </div>
    
    <!-- Mobile Menu with Svelte Transition -->
    {#if mobileMenuOpen}
      <div transition:fly={{ y: -20, duration: 300 }} class="md:hidden px-6 pb-4 flex flex-col items-start">
        <a href="#about" on:click={() => mobileMenuOpen = false} class="block py-2 hover:text-sky-400 transition-colors">About</a>
        <a href="#services" on:click={() => mobileMenuOpen = false} class="block py-2 hover:text-sky-400 transition-colors">Services</a>
        <a href="#approach" on:click={() => mobileMenuOpen = false} class="block py-2 hover:text-sky-400 transition-colors">Approach</a>
        <a href="#skills" on:click={() => mobileMenuOpen = false} class="block py-2 hover:text-sky-400 transition-colors">Skills</a>
        <a href="#contact" on:click={() => mobileMenuOpen = false} class="block py-2 hover:text-sky-400 transition-colors">Contact</a>
        <a href="https://github.com/ahmed-cmyk" target="_blank" rel="noopener noreferrer" class="block w-full mt-2 bg-slate-800 hover:bg-slate-700 text-white font-semibold py-2 px-4 rounded-lg transition-colors text-center">GitHub</a>
      </div>
    {/if}
  </header>

  <main class="container mx-auto px-6 pt-24">
    <!-- Hero Section -->
    <section id="hero" class="min-h-[80vh] flex items-center">
      {#if heroContentLoaded}
        <div class="max-w-3xl">
          <div in:fly={{ y: 50, duration: 800, delay: 200 }}>
            <h1 class="text-5xl md:text-7xl font-bold leading-tight">
              <span class="gradient-text">Backend & Full-Stack Developer</span>
              <br />
              Building the Engine of the Web.
            </h1>
          </div>
          <div in:fly={{ y: 50, duration: 800, delay: 400 }}>
            <p class="mt-6 text-lg md:text-xl text-slate-400">
              Hi, I'm Ahmed. I specialize in building robust, scalable, and efficient backend systems that power modern web applications. Let's build something great together.
            </p>
          </div>
          <div in:fly={{ y: 50, duration: 800, delay: 600 }} class="mt-8 flex gap-4">
            <a href="#contact" class="bg-sky-500 hover:bg-sky-600 text-white font-semibold py-3 px-6 rounded-lg transition-transform transform hover:scale-105">Get In Touch</a>
            <a href="#services" class="bg-slate-800 hover:bg-slate-700 text-white font-semibold py-3 px-6 rounded-lg transition-transform transform hover:scale-105">My Services</a>
          </div>
        </div>
      {/if}
    </section>

    <!-- About Section: The section tag is always present for the observer -->
    <section id="about" class="py-20">
      <!-- The content inside is rendered conditionally with an animation -->
      {#if aboutVisible}
        <div in:fade={{ duration: 1000 }}>
          <h2 class="text-4xl font-bold text-center mb-12 section-title">About Me</h2>
          <div class="max-w-4xl mx-auto text-center text-slate-300 text-lg">
            <p class="mb-4">
                I'm a developer with a passion for building things that live on the internet. My core expertise lies in backend development, where I enjoy the challenge of architecting systems that are both powerful and efficient. I have a strong foundation in both server-side logic and database management.
            </p>
            <p>
                I am always eager to learn, contribute to open-source, and collaborate with like-minded individuals to solve complex problems. I'm currently seeking freelance opportunities to help businesses achieve their goals through technology.
            </p>
          </div>
        </div>
      {/if}
    </section>

    <!-- NEW Services Section -->
    <section id="services" class="py-20">
        {#if servicesVisible}
            <div in:fade={{ duration: 1000 }}>
                <h2 class="text-4xl font-bold text-center mb-12">What I Offer</h2>
                <div class="max-w-5xl mx-auto grid md:grid-cols-2 gap-8">
                    <div class="card p-8">
                        <h3 class="text-2xl font-bold mb-3 gradient-text">Custom Backend Development</h3>
                        <p class="text-slate-400">I build robust, secure, and scalable server-side applications tailored to your business needs using modern frameworks like Django and Laravel.</p>
                    </div>
                    <div class="card p-8">
                        <h3 class="text-2xl font-bold mb-3 gradient-text">API Design & Integration</h3>
                        <p class="text-slate-400">I design and develop clean, efficient RESTful APIs to connect your services, mobile applications, and third-party tools seamlessly.</p>
                    </div>
                    <div class="card p-8">
                        <h3 class="text-2xl font-bold mb-3 gradient-text">Database Architecture</h3>
                        <p class="text-slate-400">I specialize in designing and managing optimized, scalable database schemas with both SQL and NoSQL solutions to ensure data integrity and performance.</p>
                    </div>
                    <div class="card p-8">
                        <h3 class="text-2xl font-bold mb-3 gradient-text">Full-Stack Collaboration</h3>
                        <p class="text-slate-400">While my focus is backend, I work closely with frontend developers to ensure smooth integration and a cohesive, high-performing final product.</p>
                    </div>
                </div>
            </div>
        {/if}
    </section>

    <!-- NEW Approach Section -->
    <section id="approach" class="py-20">
        {#if approachVisible}
            <div in:fade={{ duration: 1000 }}>
                <h2 class="text-4xl font-bold text-center mb-12">My Development Approach</h2>
                <div class="max-w-4xl mx-auto text-center text-slate-300 text-lg">
                    <p class="mb-4">
                        <strong class="text-slate-100">Clean & Maintainable Code:</strong> I believe that code is read more often than it is written. I prioritize writing clean, well-documented, and maintainable code that is easy for teams to collaborate on.
                    </p>
                    <p class="mb-4">
                         <strong class="text-slate-100">Performance First:</strong> A slow application is a broken application. I focus on writing efficient queries, optimizing logic, and designing systems that scale gracefully under load.
                    </p>
                    <p>
                         <strong class="text-slate-100">Clear Communication:</strong> I believe the best results come from transparent and continuous communication. I strive to keep stakeholders informed and work collaboratively to solve problems.
                    </p>
                </div>
            </div>
        {/if}
    </section>

    <!-- Projects Section (Commented Out) -->
    <!--
    <section id="projects" class="py-20">
      {#if projectsVisible}
        <div in:fade={{ duration: 1000, delay: 200 }}>
          <h2 class="text-4xl font-bold text-center mb-12 section-title">Featured Projects</h2>
          <div class="grid md:grid-cols-2 lg:grid-cols-3 gap-8">
            <div class="card rounded-lg overflow-hidden project-card">
              <img src="https://placehold.co/600x400/0a0a0a/38bdf8?text=Project+1" alt="Project 1" class="w-full h-48 object-cover">
              <div class="p-6">
                <h3 class="text-xl font-bold mb-2">Portfolio-V2</h3>
                <p class="text-slate-400 mb-4">My personal portfolio website built with Next.js, showcasing my skills and projects.</p>
                <div class="flex flex-wrap gap-2 mb-4">
                  <span class="bg-sky-900/50 text-sky-300 text-xs font-semibold px-2.5 py-1 rounded-full">Next.js</span>
                  <span class="bg-purple-900/50 text-purple-300 text-xs font-semibold px-2.5 py-1 rounded-full">React</span>
                  <span class="bg-green-900/50 text-green-300 text-xs font-semibold px-2.5 py-1 rounded-full">TailwindCSS</span>
                </div>
                <a href="https://github.com/ahmed-cmyk/Portfolio-V2" target="_blank" rel="noopener noreferrer" class="text-sky-400 hover:text-sky-300 font-semibold">View on GitHub &rarr;</a>
              </div>
            </div>
          </div>
        </div>
      {/if}
    </section>
    -->
    
    <!-- Skills Section -->
    <section id="skills" class="py-20">
      {#if skillsVisible}
        <div in:fade={{ duration: 1000, delay: 200 }}>
          <h2 class="text-4xl font-bold text-center mb-12 section-title">My Tech Stack</h2>
          <div class="max-w-4xl mx-auto">
            <div class="flex flex-wrap justify-center items-center gap-4 md:gap-8">
              <div class="skill-item flex items-center gap-3 bg-slate-800/50 px-4 py-2 rounded-lg">
                  <img src="https://www.vectorlogo.zone/logos/python/python-icon.svg" alt="Python" class="h-8 w-8">
                  <span class="font-semibold">Python</span>
              </div>
              <div class="skill-item flex items-center gap-3 bg-slate-800/50 px-4 py-2 rounded-lg">
                  <img src="https://www.vectorlogo.zone/logos/djangoproject/djangoproject-icon.svg" alt="Django" class="h-8 w-8">
                  <span class="font-semibold">Django</span>
              </div>
                <div class="skill-item flex items-center gap-3 bg-slate-800/50 px-4 py-2 rounded-lg">
                  <img src="https://www.vectorlogo.zone/logos/laravel/laravel-icon.svg" alt="Laravel" class="h-8 w-8">
                  <span class="font-semibold">Laravel</span>
              </div>
              <div class="skill-item flex items-center gap-3 bg-slate-800/50 px-4 py-2 rounded-lg">
                  <img src="https://www.vectorlogo.zone/logos/nodejs/nodejs-icon.svg" alt="Node.js" class="h-8 w-8">
                  <span class="font-semibold">Node.js</span>
              </div>
              <div class="skill-item flex items-center gap-3 bg-slate-800/50 px-4 py-2 rounded-lg">
                  <img src="https://www.vectorlogo.zone/logos/javascript/javascript-icon.svg" alt="JavaScript" class="h-8 w-8">
                  <span class="font-semibold">JavaScript</span>
              </div>
              <div class="skill-item flex items-center gap-3 bg-slate-800/50 px-4 py-2 rounded-lg">
                  <img src="https://www.vectorlogo.zone/logos/reactjs/reactjs-icon.svg" alt="React" class="h-8 w-8">
                  <span class="font-semibold">React</span>
              </div>
              <div class="skill-item flex items-center gap-3 bg-slate-800/50 px-4 py-2 rounded-lg">
                  <img src="https://www.vectorlogo.zone/logos/mongodb/mongodb-icon.svg" alt="MongoDB" class="h-8 w-8">
                  <span class="font-semibold">MongoDB</span>
              </div>
              <div class="skill-item flex items-center gap-3 bg-slate-800/50 px-4 py-2 rounded-lg">
                  <img src="https://www.vectorlogo.zone/logos/tailwindcss/tailwindcss-icon.svg" alt="Tailwind CSS" class="h-8 w-8">
                  <span class="font-semibold">Tailwind CSS</span>
              </div>
            </div>
          </div>
        </div>
      {/if}
    </section>

    <!-- Contact Section -->
    <section id="contact" class="py-20 text-center">
      {#if contactVisible}
        <div in:fade={{ duration: 1000, delay: 200 }}>
          <h2 class="text-4xl font-bold text-center mb-4 section-title">Let's Build Something</h2>
          <p class="max-w-2xl mx-auto text-slate-400 mb-8">
            Have an idea or a project you need help with? I'm currently available for freelance opportunities. Let's connect and discuss how I can bring value to your team.
          </p>
          <a href="mailto:ikramahmed398@gmail.com" class="inline-block bg-sky-500 hover:bg-sky-600 text-white font-semibold py-3 px-8 rounded-lg transition-transform transform hover:scale-105">
            Say Hello
          </a>
        </div>
      {/if}
    </section>
  </main>

  <footer class="text-center py-8 text-slate-500 border-t border-slate-800">
    <p>&copy; {new Date().getFullYear()} Ahmed. All rights reserved.</p>
  </footer>
</div>

<style>
  /* These global styles will be applied. In a SvelteKit project, you would typically
     place these in a `src/app.css` file and import it in your main layout. */
  :global(body) {
    font-family: 'Inter', sans-serif;
    background-color: #0a0a0a;
    color: #e2e8f0;
    scroll-behavior: smooth;
  }

  .gradient-text {
    background: linear-gradient(90deg, #38bdf8, #818cf8, #c084fc);
    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent;
  }
  .card {
    background-color: #1e1e1e;
    border: 1px solid #2d2d2d;
    transition: transform 0.3s ease, box-shadow 0.3s ease;
  }
  .card:hover {
    transform: translateY(-5px);
    box-shadow: 0 10px 15px -3px rgba(56, 189, 248, 0.1);
  }
</style>
