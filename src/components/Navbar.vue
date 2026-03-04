<template>
    <nav class="navbar navbar-expand-lg navbar-light fixed-top bg-navbar">
        <div class="container">
            <button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#navbarNav" aria-controls="navbarNav" aria-expanded="false" aria-label="Toggle navigation">
                <span class="navbar-toggler-icon"></span>
            </button>
            
            <div class="collapse navbar-collapse justify-content-center" id="navbarNav">
                <ul class="navbar-nav">
                    <li class="nav-item mx-3">
                        <a class="nav-link" href="#landing">Home</a>
                    </li>
                    <li class="nav-item mx-3">
                        <a class="nav-link" href="#projects">My Projects</a>
                    </li>
                    <li class="nav-item mx-3">
                        <a class="nav-link" href="#tools">Tools</a>
                    </li>
                    <li class="nav-item mx-3">
                        <a class="nav-link" href="#contact">Contact</a>
                    </li>
                </ul>
            </div>
        </div>
    </nav>
</template>

<style scoped>
.bg-navbar {
  background-color: #ffffff;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}
</style>