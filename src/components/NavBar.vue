<script setup lang="ts">

import { ref } from 'vue'

// Reactive state to track menu visibility
const isMenuOpen = ref(false)

// Function to toggle the menu state
const toggleMenu = () => {
  isMenuOpen.value = !isMenuOpen.value
}

</script>

<template>
    <!-- <nav class="navbar">
        <div class="page-heading">
            <div class="page-title">
                <router-link to="/">
                    <img src="/Makeup%20Productslogo.png" alt="Makeup Products logo" class="page-logo" />
                </router-link>
            </div>
            <p>Browse all products, 10 items per page, with simple page navigation.</p>
        </div>
    </nav> -->

  <nav class="navbar">
    <div class="navbar-container">
      <!-- Logo -->
      <div class="navbar-logo">
        <a href="#">BrandLogo</a>
      </div>

      <!-- Hamburger Menu Button -->
      <button 
        class="menu-toggle" 
        :class="{ 'is-active': isMenuOpen }" 
        @click="toggleMenu"
        aria-label="Toggle navigation"
      >
        <span class="bar"></span>
        <span class="bar"></span>
        <span class="bar"></span>
      </button>

      <!-- Navigation Links -->
      <ul class="navbar-links" :class="{ 'show-menu': isMenuOpen }">
        <router-link to="/">
          <li><a href="/" class="nav-link">Home</a></li>
        </router-link>
        <router-link to="/types">
        <li><a href="/types" class="nav-link">Types</a></li>
        </router-link>
        <li><a href="/about" class="nav-link">About</a></li>
        <li><a href="/contact" class="nav-link">Contact</a></li>
        <li><a href="/login" class="nav-btn">Login</a></li>
      </ul>
    </div>
  </nav>
    
</template>


<style scoped>
.page-logo {
    max-width: 260px;
    height: auto;
}

.page-heading h1 {
    margin: 0;
    font-size: clamp(2rem, 2.4vw, 2.4rem);
    color: var(--primary-4);
}

.page-heading p {
    margin: 10px 0 0;
    color: var(--text-muted);
    max-width: 560px;
}

.page-title {
    display: flex;
    align-items: center;
    gap: 18px;
}

.page-heading {
    display: flex;
    justify-content: space-between;
    align-items: center;
    gap: 18px;
    flex-wrap: wrap;
    margin-bottom: 26px;
    padding: 24px 24px 18px;
    background: rgba(230, 73, 140, 0.08);
    border: 1px solid rgba(230, 73, 140, 0.14);
    border-radius: 24px;
}
.summary {
    color: var(--primary-2);
    font-weight: 700;
    background: rgba(230, 109, 73, 0.12);
    border-radius: 999px;
    padding: 10px 16px;
}

/* Main Navbar Styling */
.navbar {
  background-color: #ffffff;
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  z-index: 1000;
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
  direction: ltr; /* Left-to-Right orientation */
}

.navbar-container {
  display: flex;
  justify-content: space-between;
  align-items: center;
  max-width: 1200px;
  margin: 0 auto;
  padding: 15px 20px;
}

.navbar-logo a {
  font-size: 24px;
  font-weight: bold;
  color: #3498db;
  text-decoration: none;
}

.navbar-links {
  display: flex;
  list-style: none;
  align-items: center;
  margin: 0;
  padding: 0;
}

.navbar-links li {
  margin-left: 20px;
}

.nav-link {
  text-decoration: none;
  color: #333;
  font-size: 16px;
  font-weight: 500;
  transition: color 0.3s ease;
}

.nav-link:hover, .nav-link.active {
  color: #3498db;
}

/* Login Button Styling */
.nav-btn {
  text-decoration: none;
  background-color: #3498db;
  color: #fff;
  padding: 8px 18px;
  border-radius: 5px;
  transition: background-color 0.3s ease;
}

.nav-btn:hover {
  background-color: #2980b9;
}

/* Hamburger Icon Styling */
.menu-toggle {
  display: none;
  flex-direction: column;
  justify-content: space-between;
  width: 30px;
  height: 21px;
  background: transparent;
  border: none;
  cursor: pointer;
  padding: 0;
}

.menu-toggle .bar {
  height: 3px;
  width: 100%;
  background-color: #333;
  border-radius: 10px;
  transition: all 0.3s ease;
}

/* Responsive Design for Mobile Devices */
@media (max-width: 768px) {
  .menu-toggle {
    display: flex;
  }

  .navbar-links {
    position: absolute;
    top: 60px;
    left: -100%; /* Hidden off-screen by default */
    flex-direction: column;
    background-color: #ffffff;
    width: 100%;
    text-align: center;
    transition: 0.4s ease;
    box-shadow: 0 10px 10px rgba(0, 0, 0, 0.05);
    padding: 20px 0;
  }

  .navbar-links.show-menu {
    left: 0; /* Slide in menu */
  }

  .navbar-links li {
    margin: 15px 0;
    margin-left: 0;
  }

  /* Animate Hamburger Icon into an 'X' */
  .menu-toggle.is-active .bar:nth-child(1) {
    transform: translateY(9px) rotate(45deg);
  }

  .menu-toggle.is-active .bar:nth-child(2) {
    opacity: 0;
  }

  .menu-toggle.is-active .bar:nth-child(3) {
    transform: translateY(-9px) rotate(-45deg);
  }
}
</style>