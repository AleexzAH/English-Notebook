
<template>
    <header>
        <nav class="menu">
            <ul>
                <li><a href="#">Caderno Principal</a></li>
                <li class="dropdown">
                    <span>Vocabulário</span>
                    <ul class="sub-menu">
                        <li><a href="#">Partes do corpo, família e objetos</a></li>
                        <li><a href="#">Animais e meios de transporte</a></li>
                        <li><a href="#">Sentimentos e sensações</a></li>
                    </ul>
                </li>
                <li><a href="#">Fonética</a></li>
            </ul>
        </nav>
    </header>
</template>

<style>
* {
    padding: 0;
    margin: 0;
    box-sizing: border-box;
}

header {
    min-width: 100%;
}
.menu ul {
    display: flex;
    justify-content: center;
    gap: 15%;
}

.menu ul li {
    position: relative;
    list-style: none;
    cursor: pointer;
    padding: 10px;
}
ul li a {
    text-decoration: none;
    color: black;
}

/* Submenu escondido inicialmente */
.sub-menu {
    position: absolute;
    top: 100%;
    left: 10px;
    display: flex;
    flex-direction: column;
    opacity: 0;
    transform: translateY(10px);
    pointer-events: none;
    transition: all 0.3s ease;
}

.sub-menu li {
    padding: 10px;
    background: #eee;
    width: 300px;
}
.sub-menu li:hover {
    opacity: 100;
}

/* Quando passa o mouse no pai */
.dropdown:hover .sub-menu, .sub-menu:hover {
    opacity: 1;
    transform: translateY(0);
    pointer-events: auto;
}
</style>