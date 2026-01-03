<script setup lang="ts">
import { ref, onMounted, onUnmounted } from 'vue';
import type { Ref } from 'vue';
import { 
  Github, 
  Linkedin, 
  Mail, 
  Code, 
  Briefcase, 
  ExternalLink, 
  Terminal, 
  Database, 
  Cpu, 
  Server, 
  Layers, 
  Flame,
  Sun,
  Moon,
  Menu,
  X,
  ArrowUp,
  Send,
  Hexagon,
  Hash
} from "lucide-vue-next";

const name = "Indra Mochamad Farros";
const roles = ["Software Engineering"];
const currentRole: Ref<string> = ref("");
const isDark: Ref<boolean> = ref(true);
const isMenuOpen: Ref<boolean> = ref(false);
const isScrolled: Ref<boolean> = ref(false);
const showBackToTop: Ref<boolean> = ref(false);
const selectedProject: Ref<Project | null> = ref(null);
const isModalOpen: Ref<boolean> = ref(false);

// Types
interface Skill {
  name: string;
  icon: any;
  color: string;
}

interface TimelineItem {
  year: string;
  title: string;
  company: string;
  description: string;
}

interface Project {
  title: string;
  description: string;
  tags: string[];
  link: string;
}

// Typing Effect Logic
let roleIndex = 0;
let charIndex = 0;
let isDeleting = false;
let typeTimeout: ReturnType<typeof setTimeout> | null = null;

const openModal = (project: Project) => {
  selectedProject.value = project;
  isModalOpen.value = true;
  document.body.style.overflow = 'hidden';
};

const closeModal = () => {
  isModalOpen.value = false;
  setTimeout(() => {
    selectedProject.value = null;
  }, 300);
  document.body.style.overflow = '';
};

const typeEffect = () => {
  const currentFullRole = roles[roleIndex];
  
  if (isDeleting) {
    currentRole.value = currentFullRole.substring(0, charIndex - 1);
    charIndex--;
  } else {
    currentRole.value = currentFullRole.substring(0, charIndex + 1);
    charIndex++;
  }

  let typeSpeed = isDeleting ? 50 : 100;

  if (!isDeleting && charIndex === currentFullRole.length) {
    isDeleting = true;
    typeSpeed = 2000; // Pause at end
  } else if (isDeleting && charIndex === 0) {
    isDeleting = false;
    roleIndex = (roleIndex + 1) % roles.length;
    typeSpeed = 500; // Pause before typing next
  }

  typeTimeout = setTimeout(typeEffect, typeSpeed);
};

const toggleTheme = () => {
  isDark.value = !isDark.value;
  updateTheme();
};

const updateTheme = () => {
  if (isDark.value) {
    document.documentElement.classList.add('dark');
    localStorage.setItem('theme', 'dark');
  } else {
    document.documentElement.classList.remove('dark');
    localStorage.setItem('theme', 'light');
  }
};

const handleScroll = () => {
  isScrolled.value = window.scrollY > 50;
  showBackToTop.value = window.scrollY > 500;
};

const scrollToTop = () => {
  window.scrollTo({ top: 0, behavior: 'smooth' });
};

onMounted(() => {
  const savedTheme = localStorage.getItem('theme');
  if (savedTheme) {
    isDark.value = savedTheme === 'dark';
  } else {
    isDark.value = window.matchMedia('(prefers-color-scheme: dark)').matches;
  }
  updateTheme();
  
  // Start typing effect
  typeEffect();
  
  // Scroll listener
  window.addEventListener('scroll', handleScroll);
});

onUnmounted(() => {
  if (typeTimeout) clearTimeout(typeTimeout);
  window.removeEventListener('scroll', handleScroll);
});

const skills: Skill[] = [
  { name: "Golang", icon: Terminal, color: "text-orange-600 dark:text-orange-500" },
  { name: "Node.js", icon: Server, color: "text-zinc-700 dark:text-zinc-400" },
  { name: "Vue.js", icon: Layers, color: "text-zinc-700 dark:text-zinc-400" },
  { name: "Laravel", icon: Server, color: "text-orange-600 dark:text-orange-500" },
  { name: "NestJS", icon: Hexagon, color: "text-zinc-700 dark:text-zinc-400" },
  { name: "Express.js", icon: Hash, color: "text-slate-600 dark:text-slate-400" },
  { name: "CodeIgniter", icon: Flame, color: "text-orange-600 dark:text-orange-500" },
  { name: "PostgreSQL", icon: Database, color: "text-zinc-700 dark:text-zinc-400" },
  { name: "MySQL", icon: Database, color: "text-orange-600 dark:text-orange-400" },
  { name: "System Design", icon: Cpu, color: "text-zinc-700 dark:text-zinc-400" },
];

const timeline: TimelineItem[] = [
  {
    year: "Mar 2023 - Present",
    title: "Software Engineering",
    company: "PT EDI Indonesia",
    description: "Developed and maintained multiple enterprise applications including a custom Helpdesk system, B2B billing synchronization, and Customs (Bea Cukai) integrations. Also optimized critical logistics platforms such as Airport TPS Online."
  },
  {
    year: "Juni 2021 - Dec 2022",
    title: "Web Developer",
    company: "Migen Software House",
    description: "Built custom web solutions for clients, collaborating with cross-functional teams to deliver high-quality software."
  },
  {
    year: "Juli 2020 - Agustus 2020",
    title: "Junior Web Developer",
    company: "PT Penerbit Erlangga",
    description: "Assisted in the development of internal web tools and maintained existing web platforms."
  },
  {
    year: "2018",
    title: "Access Data Management (Internship)",
    company: "Telkom Indonesia",
    description: "Managed data access protocols and assisted in network infrastructure monitoring during internship."
  }
];

const projects: Project[] = [
  {
    title: "sAirport TPS Online System",
    description: "Integrated logistics platform for airport temporary storage, featuring real-time Customs (Bea Cukai) data exchange and automated B2B billing.",
    tags: ["Integration", "Customs API", "Logistics"],
    link: "#"
  },
  {
    title: "Agent Ticket Booking Platform",
    description: "B2B platform for agents to book tickets, manage schedules, and handle payments with customer support integration.",
    tags: ["Codeigniter", "MySQL", "Payment Gateway"],
    link: "#"
  },
  {
    title: "Hotel Booking Integration",
    description: "Developed a hotel booking module featuring seamless payment API integration and a user-friendly booking interface.",
    tags: ["API Integration", "Payment Gateway", "Booking System"],
    link: "#"
  },
  {
    title: "Gold & Forex POS System",
    description: "Specialized Point of Sale system for gold and forex stores featuring operational optimization and financial reporting.",
    tags: ["Codeigniter", "MySQL", "Reporting"],
    link: "#"
  },
  {
    title: "PJU Production Application",
    description: "Management system for public street lighting pole production, facilitating progress tracking and coordination.",
    tags: ["Codeigniter", "MySQL", "Production"],
    link: "#"
  },
  {
    title: "Logistics Container Module",
    description: "Developed a container return module to enhance logistics process efficiency and tracking.",
    tags: ["PHP", "Logistics", "Optimization"],
    link: "#"
  },
  {
    title: "Beauty Clinic Management",
    description: "Maintenance and feature expansion for a beauty clinic application to optimize operational functionality.",
    tags: ["CodeIgniter", "Maintenance", "Healthcare"],
    link: "#"
  },
  {
    title: "Enterprise App Maintenance",
    description: "Regular maintenance and optimization of existing running applications to ensure stability and performance.",
    tags: ["Maintenance", "Bug Fixes", "Optimization"],
    link: "#"
  },
  {
    title: "Ministry LHKPN System",
    description: "Developed a wealth reporting system (LHKPN) for a government ministry to ensure transparency and compliance.",
    tags: ["Laravel", "MySQL"],
    link: "#"
  },
  {
    title: "Stunting Awareness Portal",
    description: "Educational landing page for a community health center (Puskesmas) focused on stunting prevention and nutrition.",
    tags: ["Frontend", "Public Health", "Responsive"],
    link: "#"
  }
];
</script>

<template>
  <div class="min-h-screen bg-zinc-100 dark:bg-zinc-950 text-zinc-900 dark:text-zinc-300 font-sans selection:bg-orange-500/30 selection:text-orange-100 overflow-x-hidden transition-colors duration-300">
    
    <!-- Navbar -->
    <nav 
      :class="[
        'fixed top-0 left-0 right-0 z-50 transition-all duration-300 border-b-4',
        isScrolled 
          ? 'bg-white/95 dark:bg-zinc-950/95 backdrop-blur-sm border-orange-500 py-3 shadow-[0_4px_0_0_rgba(0,0,0,0.1)]' 
          : 'bg-transparent border-transparent py-6'
      ]"
    >
      <div class="container mx-auto px-6 flex justify-between items-center max-w-6xl">
        <a href="#" class="text-2xl font-black text-zinc-900 dark:text-white uppercase tracking-tighter flex items-center gap-2">
          <div class="w-2 h-8 bg-orange-500"></div>
          IF
        </a>

        <!-- Desktop Menu -->
        <div class="hidden md:flex items-center gap-1">
          <a href="#home" class="text-xs font-black uppercase tracking-wider px-4 py-2 hover:bg-orange-500 hover:text-white transition-all">Home</a>
          <a href="#tech" class="text-xs font-black uppercase tracking-wider px-4 py-2 hover:bg-orange-500 hover:text-white transition-all">Tech</a>
          <a href="#journey" class="text-xs font-black uppercase tracking-wider px-4 py-2 hover:bg-orange-500 hover:text-white transition-all">Journey</a>
          <a href="#projects" class="text-xs font-black uppercase tracking-wider px-4 py-2 hover:bg-orange-500 hover:text-white transition-all">Projects</a>
          
          <!-- Theme Toggle -->
          <button 
            @click="toggleTheme" 
            class="ml-4 p-2 bg-zinc-900 dark:bg-zinc-100 hover:bg-orange-500 dark:hover:bg-orange-500 transition-colors text-white dark:text-zinc-900 hover:text-white"
            aria-label="Toggle Theme"
          >
            <Sun v-if="!isDark" :size="18" />
            <Moon v-else :size="18" />
          </button>
        </div>

        <!-- Mobile Menu Button -->
        <button @click="isMenuOpen = !isMenuOpen" class="md:hidden text-zinc-900 dark:text-white">
          <Menu v-if="!isMenuOpen" />
          <X v-else />
        </button>
      </div>

      <!-- Mobile Menu Dropdown -->
      <div v-if="isMenuOpen" class="md:hidden absolute top-full left-0 right-0 bg-white dark:bg-zinc-950 border-b-4 border-orange-500 p-6 flex flex-col gap-2 shadow-2xl">
        <a href="#home" @click="isMenuOpen = false" class="text-sm font-black uppercase tracking-wider px-4 py-3 hover:bg-orange-500 hover:text-white transition-all border-l-4 border-transparent hover:border-zinc-900 dark:hover:border-white">Home</a>
        <a href="#tech" @click="isMenuOpen = false" class="text-sm font-black uppercase tracking-wider px-4 py-3 hover:bg-orange-500 hover:text-white transition-all border-l-4 border-transparent hover:border-zinc-900 dark:hover:border-white">Tech</a>
        <a href="#journey" @click="isMenuOpen = false" class="text-sm font-black uppercase tracking-wider px-4 py-3 hover:bg-orange-500 hover:text-white transition-all border-l-4 border-transparent hover:border-zinc-900 dark:hover:border-white">Journey</a>
        <a href="#projects" @click="isMenuOpen = false" class="text-sm font-black uppercase tracking-wider px-4 py-3 hover:bg-orange-500 hover:text-white transition-all border-l-4 border-transparent hover:border-zinc-900 dark:hover:border-white">Projects</a>
        <div class="flex items-center justify-between mt-4 pt-4 border-t-2 border-zinc-300 dark:border-zinc-800">
          <span class="text-xs font-black uppercase tracking-wider">Theme</span>
          <button 
            @click="toggleTheme" 
            class="p-2 bg-zinc-900 dark:bg-zinc-100 text-white dark:text-zinc-900"
          >
            <Sun v-if="!isDark" :size="20" />
            <Moon v-else :size="20" />
          </button>
        </div>
      </div>
    </nav>

    <!-- Industrial Background Grid -->
    <div class="fixed inset-0 z-0 pointer-events-none opacity-20 dark:opacity-10 transition-opacity duration-500" 
         style="background-image: linear-gradient(rgba(0, 0, 0, 0.05) 2px, transparent 2px), linear-gradient(90deg, rgba(0, 0, 0, 0.05) 2px, transparent 2px); background-size: 40px 40px;">
    </div>
    
    <!-- Diagonal Lines Pattern -->
    <div class="fixed inset-0 z-0 pointer-events-none opacity-5 dark:opacity-10" 
         style="background-image: repeating-linear-gradient(45deg, transparent, transparent 35px, rgba(255, 140, 0, 0.1) 35px, rgba(255, 140, 0, 0.1) 37px);">
    </div>

    <div class="relative z-10 container mx-auto px-6 py-12 max-w-6xl pt-32">
      
      <!-- Hero Section -->
      <header 
        id="home"
        v-motion
        :initial="{ opacity: 0, y: 50 }"
        :enter="{ opacity: 1, y: 0, transition: { duration: 800, ease: 'easeOut' } }"
        class="mb-32 pt-12"
      >
        <div class="grid md:grid-cols-[300px_1fr] gap-8 mb-12 items-start">
          <!-- Photo Section -->
          <div 
            v-motion
            :initial="{ opacity: 0, x: -50 }"
            :enter="{ opacity: 1, x: 0, transition: { duration: 800, delay: 200 } }"
            class="relative order-2 md:order-1 w-full max-w-[280px] mx-auto md:mx-0"
          >
            <div class="relative">
              <!-- Industrial frame -->
              <div class="absolute -inset-2 border-4 border-zinc-900 dark:border-white"></div>
              <div class="absolute -top-3 -right-3 w-12 h-12 bg-orange-500"></div>
              <div class="absolute -bottom-3 -left-3 w-12 h-12 border-4 border-orange-500"></div>
              
              <!-- Photo container -->
              <div class="relative aspect-square bg-zinc-200 dark:bg-zinc-800 overflow-hidden border-2 border-zinc-900 dark:border-white">
                <!-- Replace this with your actual photo -->
                <img 
                  src="https://ui-avatars.com/api/?name=Indra+Farros&size=400&background=f97316&color=fff&bold=true&font-size=0.4" 
                  alt="Indra Mochamad Farros"
                  class="w-full h-full object-cover grayscale hover:grayscale-0 transition-all duration-500"
                />
                
                <!-- Corner decorations -->
                <div class="absolute top-0 left-0 w-6 h-6 border-t-2 border-l-2 border-orange-500"></div>
                <div class="absolute bottom-0 right-0 w-6 h-6 border-b-2 border-r-2 border-orange-500"></div>
              </div>
              
              <!-- Stats overlay -->
              <div class="absolute bottom-4 right-4 bg-zinc-900 dark:bg-white border-2 border-orange-500 p-2">
                <div class="text-2xl font-black text-white dark:text-zinc-900">4+</div>
                <div class="text-[8px] font-black uppercase tracking-widest text-white dark:text-zinc-900">Years</div>
              </div>
            </div>
          </div>

          <!-- Brutalist Name Block -->
          <div class="relative order-1 md:order-2">
            <div class="absolute -left-4 top-0 w-2 h-full bg-orange-500"></div>
            <div class="pl-8">
              <div class="inline-block mb-4 px-4 py-1 bg-zinc-900 dark:bg-white text-white dark:text-zinc-900">
                <span class="text-xs font-black uppercase tracking-widest">Software Engineer</span>
              </div>
              <h1 class="text-5xl md:text-7xl lg:text-8xl font-black mb-4 tracking-tighter text-zinc-900 dark:text-white uppercase leading-none">
                {{ name.split(' ')[0] }}<br/>
                <span class="text-zinc-600 dark:text-zinc-500">{{ name.split(' ').slice(1).join(' ') }}</span>
              </h1>
              <div class="flex items-center gap-4 mb-8">
                <div class="h-1 w-16 bg-orange-500"></div>
                <h2 class="text-xl md:text-2xl font-mono font-bold text-zinc-700 dark:text-zinc-400 uppercase">
                  {{ currentRole }}<span class="animate-pulse text-orange-500">_</span>
                </h2>
              </div>
            </div>
          </div>
        </div>
        
        <!-- Description Grid -->
        <div class="grid md:grid-cols-3 gap-6 mb-12">
          <div class="md:col-span-2 border-l-4 border-orange-500 pl-6">
            <p class="text-lg md:text-xl text-zinc-700 dark:text-zinc-400 leading-relaxed font-light">
              Building <span class="font-bold text-zinc-900 dark:text-white">enterprise-grade systems</span> and 
              <span class="font-bold text-zinc-900 dark:text-white">high-performance applications</span>.
              <span class="block mt-4">4+ years of production experience.</span>
            </p>
          </div>
          <div class="flex flex-col gap-3">
            <div class="flex items-center gap-3 text-sm">
              <div class="w-3 h-3 bg-orange-500"></div>
              <span class="font-mono font-bold uppercase text-xs">Backend Systems</span>
            </div>
            <div class="flex items-center gap-3 text-sm">
              <div class="w-3 h-3 bg-zinc-400"></div>
              <span class="font-mono font-bold uppercase text-xs">API Development</span>
            </div>
            <div class="flex items-center gap-3 text-sm">
              <div class="w-3 h-3 bg-zinc-600"></div>
              <span class="font-mono font-bold uppercase text-xs">System Architecture</span>
            </div>
          </div>
        </div>
        
        <!-- CTA Buttons Industrial Style -->
        <div class="flex gap-4 flex-wrap mb-12">
          <a href="mailto:indrafarros.fi@gmail.com" class="group relative px-8 py-4 bg-orange-500 text-white font-black uppercase text-sm tracking-wider hover:bg-orange-600 transition-all border-b-4 border-orange-700 hover:border-b-2 hover:translate-y-0.5">
            <span class="flex items-center gap-2">
              <Mail :size="18" />
              Contact Me
            </span>
          </a>
          <a href="#projects" class="px-8 py-4 bg-zinc-900 dark:bg-white text-white dark:text-zinc-900 font-black uppercase text-sm tracking-wider hover:bg-zinc-800 dark:hover:bg-zinc-100 transition-all border-b-4 border-zinc-950 dark:border-zinc-300 hover:border-b-2 hover:translate-y-0.5">
            View Projects
          </a>
        </div>
        
        <!-- Social Links Industrial -->
        <div class="flex gap-3">
          <a href="https://github.com" target="_blank" class="p-3 bg-white dark:bg-zinc-900 border-2 border-zinc-900 dark:border-white hover:bg-orange-500 hover:border-orange-500 dark:hover:bg-orange-500 dark:hover:border-orange-500 transition-all group">
            <Github :size="20" class="text-zinc-900 dark:text-white group-hover:text-white transition-colors" />
          </a>
          <a href="https://linkedin.com/in/indrafarros" target="_blank" class="p-3 bg-white dark:bg-zinc-900 border-2 border-zinc-900 dark:border-white hover:bg-orange-500 hover:border-orange-500 dark:hover:bg-orange-500 dark:hover:border-orange-500 transition-all group">
            <Linkedin :size="20" class="text-zinc-900 dark:text-white group-hover:text-white transition-colors" />
          </a>
          <a href="mailto:indrafarros.fi@gmail.com" class="p-3 bg-white dark:bg-zinc-900 border-2 border-zinc-900 dark:border-white hover:bg-orange-500 hover:border-orange-500 dark:hover:bg-orange-500 dark:hover:border-orange-500 transition-all group">
            <Mail :size="20" class="text-zinc-900 dark:text-white group-hover:text-white transition-colors" />
          </a>
        </div>
      </header>

      <!-- Tech Stack -->
      <section id="tech" class="mb-32 scroll-mt-32">
        <div 
          v-motion
          :initial="{ opacity: 0, x: -50 }"
          :visible="{ opacity: 1, x: 0, transition: { duration: 600 } }"
          class="mb-12"
        >
          <div class="flex items-center gap-4 mb-2">
            <div class="w-2 h-12 bg-orange-500"></div>
            <h3 class="text-4xl md:text-5xl font-black text-zinc-900 dark:text-white uppercase tracking-tighter">
              Tech Stack
            </h3>
          </div>
          <div class="pl-6 border-l-4 border-zinc-300 dark:border-zinc-800 ml-2">
            <p class="text-sm font-mono uppercase tracking-wider text-zinc-600 dark:text-zinc-500">Production-Ready Technologies</p>
          </div>
        </div>
        
        <div class="grid grid-cols-2 sm:grid-cols-3 lg:grid-cols-4 xl:grid-cols-5 gap-4">
          <div v-for="(skill, index) in skills" :key="skill.name" 
               v-motion
               :initial="{ opacity: 0, y: 20 }"
               :visible="{ opacity: 1, y: 0, transition: { delay: index * 80, duration: 400 } }"
               class="group relative bg-white dark:bg-zinc-900 border-2 border-zinc-900 dark:border-white p-6 flex flex-col items-center gap-4 hover:bg-orange-500 hover:border-orange-500 transition-all cursor-default">
            <component :is="skill.icon" :size="36" :class="[skill.color, 'group-hover:text-white transition-colors duration-300']" />
            <span class="font-black text-xs uppercase tracking-wider text-zinc-900 dark:text-white group-hover:text-white transition-colors duration-300 text-center">{{ skill.name }}</span>
            <div class="absolute top-2 right-2 w-2 h-2 bg-zinc-300 dark:bg-zinc-700 group-hover:bg-white transition-colors"></div>
          </div>
        </div>
      </section>

      <!-- Experience Timeline -->
      <section id="journey" class="mb-32 scroll-mt-32">
        <div 
          v-motion
          :initial="{ opacity: 0, x: 50 }"
          :visible="{ opacity: 1, x: 0, transition: { duration: 600 } }"
          class="mb-12"
        >
          <div class="flex items-center gap-4 mb-2">
            <div class="w-2 h-12 bg-orange-500"></div>
            <h3 class="text-4xl md:text-5xl font-black text-zinc-900 dark:text-white uppercase tracking-tighter">
              Experience
            </h3>
          </div>
          <div class="pl-6 border-l-4 border-zinc-300 dark:border-zinc-800 ml-2">
            <p class="text-sm font-mono uppercase tracking-wider text-zinc-600 dark:text-zinc-500">Professional Timeline</p>
          </div>
        </div>

        <div class="space-y-6">
          <div v-for="(item, index) in timeline" :key="index" 
               v-motion
               :initial="{ opacity: 0, y: 20 }"
               :visible="{ opacity: 1, y: 0, transition: { delay: index * 150, duration: 500 } }"
               class="group relative bg-white dark:bg-zinc-900 border-l-8 border-orange-500 p-6 hover:bg-zinc-50 dark:hover:bg-zinc-800 transition-all"
          >
            <div class="flex flex-col md:flex-row md:items-start md:justify-between gap-4 mb-4">
              <div class="flex-1">
                <div class="inline-block px-3 py-1 bg-zinc-900 dark:bg-white mb-3">
                  <span class="text-xs font-black uppercase tracking-widest text-white dark:text-zinc-900">{{ item.year }}</span>
                </div>
                <h4 class="text-2xl font-black uppercase tracking-tight text-zinc-900 dark:text-white mb-2">{{ item.title }}</h4>
                <div class="flex items-center gap-2 text-orange-600 dark:text-orange-500 font-bold uppercase text-sm">
                  <Briefcase :size="16" /> {{ item.company }}
                </div>
              </div>
            </div>
            
            <p class="text-zinc-700 dark:text-zinc-400 leading-relaxed border-l-2 border-zinc-300 dark:border-zinc-700 pl-4">
              {{ item.description }}
            </p>
            
            <!-- Industrial corner accent -->
            <div class="absolute top-6 right-6 w-4 h-4 border-t-2 border-r-2 border-zinc-300 dark:border-zinc-700 group-hover:border-orange-500 transition-colors"></div>
          </div>
        </div>
      </section>

      <!-- Projects -->
      <section id="projects" class="mb-32 scroll-mt-32">
        <div 
          v-motion
          :initial="{ opacity: 0, y: 30 }"
          :visible="{ opacity: 1, y: 0, transition: { duration: 600 } }"
          class="mb-12"
        >
          <div class="flex items-center gap-4 mb-2">
            <div class="w-2 h-12 bg-orange-500"></div>
            <h3 class="text-4xl md:text-5xl font-black text-zinc-900 dark:text-white uppercase tracking-tighter">
              Projects
            </h3>
          </div>
          <div class="pl-6 border-l-4 border-zinc-300 dark:border-zinc-800 ml-2">
            <p class="text-sm font-mono uppercase tracking-wider text-zinc-600 dark:text-zinc-500">Featured Work & Implementations</p>
          </div>
        </div>

        <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6">
          <div v-for="(project, index) in projects" :key="project.title" 
               @click="openModal(project)"
               v-motion
               :initial="{ opacity: 0, y: 30 }"
               :visible="{ opacity: 1, y: 0, transition: { delay: index * 100, duration: 500 } }"
               class="group bg-white dark:bg-zinc-900 border-4 border-zinc-900 dark:border-white p-6 hover:border-orange-500 transition-all duration-300 flex flex-col cursor-pointer relative overflow-hidden">
            
            <!-- Corner decoration -->
            <div class="absolute top-0 right-0 w-12 h-12 bg-zinc-200 dark:bg-zinc-800 group-hover:bg-orange-500 transition-colors" style="clip-path: polygon(0 0, 100% 0, 100% 100%);"></div>
            
            <div class="flex justify-between items-start mb-4 relative z-10">
              <div class="p-3 bg-zinc-900 dark:bg-white text-white dark:text-zinc-900 group-hover:bg-orange-500 group-hover:text-white transition-all">
                <Layers :size="24" />
              </div>
              <button class="p-2 border-2 border-zinc-900 dark:border-white hover:bg-zinc-900 hover:text-white dark:hover:bg-white dark:hover:text-zinc-900 transition-all">
                <ExternalLink :size="16" />
              </button>
            </div>
            
            <h4 class="text-xl font-black uppercase tracking-tight text-zinc-900 dark:text-white mb-3 group-hover:text-orange-600 dark:group-hover:text-orange-500 transition-colors">{{ project.title }}</h4>
            <p class="text-zinc-700 dark:text-zinc-400 text-sm mb-6 flex-grow leading-relaxed">{{ project.description }}</p>
            
            <div class="flex flex-wrap gap-2 mt-auto">
              <span v-for="tag in project.tags" :key="tag" 
                    class="text-xs font-black uppercase tracking-wider text-zinc-900 dark:text-white bg-zinc-200 dark:bg-zinc-800 px-3 py-1 border border-zinc-900 dark:border-white">
                {{ tag }}
              </span>
            </div>
          </div>
        </div>
      </section>

      <!-- Call to Action -->
      <section class="mb-32">
        <div 
          v-motion
          :initial="{ opacity: 0, y: 30 }"
          :visible="{ opacity: 1, y: 0, transition: { duration: 600 } }"
          class="bg-zinc-900 dark:bg-white border-8 border-zinc-900 dark:border-white p-12 md:p-16 relative overflow-hidden"
        >
          <!-- Industrial pattern -->
          <div class="absolute inset-0 opacity-10" style="background-image: repeating-linear-gradient(45deg, transparent, transparent 20px, rgba(255,255,255,0.1) 20px, rgba(255,255,255,0.1) 22px);"></div>
          
          <!-- Orange accent stripes -->
          <div class="absolute top-0 left-0 w-full h-4 bg-orange-500"></div>
          <div class="absolute bottom-0 right-0 w-full h-4 bg-orange-500"></div>
          
          <div class="relative z-10">
            <div class="flex items-center gap-4 mb-8">
              <div class="w-2 h-20 bg-orange-500"></div>
              <h3 class="text-4xl md:text-6xl font-black text-white dark:text-zinc-900 uppercase tracking-tighter leading-none">
                Let's Build<br/>Together
              </h3>
            </div>
            
            <div class="max-w-2xl mb-12 border-l-4 border-orange-500 pl-6">
              <p class="text-white/90 dark:text-zinc-900/90 text-lg md:text-xl font-light leading-relaxed">
                Open for collaboration on <span class="font-bold">enterprise systems</span>, <span class="font-bold">backend architecture</span>, and <span class="font-bold">API development</span> projects.
              </p>
            </div>
            
            <div class="flex flex-col sm:flex-row gap-4">
              <a href="mailto:indrafarros.fi@gmail.com" class="inline-flex items-center justify-center gap-3 px-10 py-5 bg-orange-500 text-white font-black uppercase text-sm tracking-wider hover:bg-orange-600 transition-all border-b-4 border-orange-700 hover:border-b-2 hover:translate-y-0.5">
                <Send :size="20" /> 
                Get in Touch
              </a>
              <a href="#projects" class="inline-flex items-center justify-center gap-3 px-10 py-5 bg-white dark:bg-zinc-900 text-zinc-900 dark:text-white font-black uppercase text-sm tracking-wider hover:bg-zinc-100 dark:hover:bg-zinc-800 transition-all border-2 border-white dark:border-zinc-900">
                <Code :size="20" /> 
                View Portfolio
              </a>
            </div>
          </div>
        </div>
      </section>

      <!-- Footer -->
      <footer class="text-center py-12 border-t-4 border-orange-500">
        <div class="flex items-center justify-center gap-4 mb-4">
          <div class="w-12 h-px bg-zinc-300 dark:bg-zinc-700"></div>
          <p class="text-xs font-black uppercase tracking-widest text-zinc-600 dark:text-zinc-500">
            &copy; {{ new Date().getFullYear() }} {{ name }}
          </p>
          <div class="w-12 h-px bg-zinc-300 dark:bg-zinc-700"></div>
        </div>
      </footer>
    </div>

    <!-- Back to Top Button -->
    <button 
      @click="scrollToTop"
      :class="[
        'fixed bottom-8 right-8 p-4 bg-orange-500 text-white shadow-lg hover:bg-orange-600 transition-all duration-300 z-50 border-4 border-zinc-900 dark:border-white',
        showBackToTop ? 'translate-y-0 opacity-100' : 'translate-y-20 opacity-0'
      ]"
    >
      <ArrowUp :size="24" />
    </button>

    <!-- Project Modal -->
    <Transition
      enter-active-class="transition duration-300 ease-out"
      enter-from-class="opacity-0"
      enter-to-class="opacity-100"
      leave-active-class="transition duration-200 ease-in"
      leave-from-class="opacity-100"
      leave-to-class="opacity-0"
    >
      <div 
        v-if="isModalOpen" 
        class="fixed inset-0 z-[100] flex items-center justify-center p-4 sm:p-6 bg-slate-900/60 backdrop-blur-sm" 
        role="dialog" 
        aria-modal="true"
        @click.self="closeModal"
      >
        <!-- Modal Content -->
        <div 
          class="relative w-full max-w-3xl bg-white dark:bg-zinc-900 shadow-2xl overflow-hidden border-8 border-zinc-900 dark:border-white flex flex-col max-h-[90vh]"
        >
          <!-- Header -->
          <div class="h-32 sm:h-48 bg-zinc-900 dark:bg-white flex items-center justify-center relative overflow-hidden">
             <div class="absolute inset-0 opacity-10" style="background-image: repeating-linear-gradient(45deg, transparent, transparent 20px, rgba(255,255,255,0.1) 20px, rgba(255,255,255,0.1) 22px);"></div>
             <Layers :size="64" class="text-white dark:text-zinc-900 relative z-10" />
             
             <button @click="closeModal" class="absolute top-4 right-4 p-3 bg-orange-500 text-white hover:bg-orange-600 transition-colors z-20">
               <X :size="20" />
             </button>
             
             <!-- Orange accent -->
             <div class="absolute bottom-0 left-0 w-full h-2 bg-orange-500"></div>
          </div>

          <!-- Content -->
          <div class="p-6 sm:p-8 overflow-y-auto">
            <div class="flex items-start gap-4 mb-6">
              <div class="w-2 h-16 bg-orange-500 flex-shrink-0"></div>
              <h3 class="text-2xl sm:text-4xl font-black uppercase tracking-tighter text-zinc-900 dark:text-white">{{ selectedProject?.title }}</h3>
            </div>
            
            <div class="flex flex-wrap gap-2 mb-6 pl-6">
              <span v-for="tag in selectedProject?.tags" :key="tag" 
                    class="text-xs font-black uppercase tracking-wider text-zinc-900 dark:text-white bg-zinc-200 dark:bg-zinc-800 px-3 py-1.5 border-2 border-zinc-900 dark:border-white">
                {{ tag }}
              </span>
            </div>

            <div class="border-l-4 border-zinc-300 dark:border-zinc-700 pl-6 mb-6">
              <p class="text-zinc-700 dark:text-zinc-400 leading-relaxed mb-4">{{ selectedProject?.description }}</p>
              <p class="text-zinc-700 dark:text-zinc-400 leading-relaxed">
                This project demonstrates the implementation of scalable architecture and modern development practices. 
                It features a robust backend, responsive frontend, and seamless integration with third-party services.
              </p>
            </div>

            <div class="mt-8 flex flex-col sm:flex-row gap-4">
               <a :href="selectedProject?.link" target="_blank" class="flex-1 flex items-center justify-center gap-2 px-8 py-4 bg-orange-500 text-white font-black uppercase text-sm tracking-wider hover:bg-orange-600 transition-all border-b-4 border-orange-700 hover:border-b-2 hover:translate-y-0.5">
                 <ExternalLink :size="18" /> View Project
               </a>
               <button @click="closeModal" class="px-8 py-4 bg-zinc-900 dark:bg-white text-white dark:text-zinc-900 font-black uppercase text-sm tracking-wider hover:bg-zinc-800 dark:hover:bg-zinc-100 transition-all border-2 border-zinc-900 dark:border-white">
                 Close
               </button>
            </div>
          </div>
        </div>
      </div>
    </Transition>
  </div>
</template>

<style>
:root {
  --scrollbar-bg: #f8fafc;
  --scrollbar-thumb: #cbd5e1;
  --scrollbar-thumb-hover: #94a3b8;
}

:root.dark {
  --scrollbar-bg: #020617;
  --scrollbar-thumb: #1e293b;
  --scrollbar-thumb-hover: #334155;
}

/* Custom scrollbar */
::-webkit-scrollbar {
  width: 8px;
}
::-webkit-scrollbar-track {
  background: var(--scrollbar-bg); 
}
::-webkit-scrollbar-thumb {
  background: var(--scrollbar-thumb); 
  border-radius: 4px;
}
::-webkit-scrollbar-thumb:hover {
  background: var(--scrollbar-thumb-hover); 
}
</style>
