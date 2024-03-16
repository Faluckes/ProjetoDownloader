<template>
<div>
    <nav>
                <a class="logo" href="/">Falowd</a>

                <div @click="showNav" class="mobile-menu">
                    <div class="line1"></div>
                    <div class="line2"></div>
                    <div class="line3"></div>
                </div>


                <ul class="nav-list">
                    <li class="nav-item"><a href="https://github.com/faluckes" target="_blank">GitHub</a></li>
                    <li class="nav-item"><a href="https://twitter.com/FaluckesNew" target="_blank">Twitter</a></li>
                </ul>
            </nav>
        <main></main>
</div>
</template>

<script>

export default {
    name: "navBar",
    methods: {
        showNav() {
            class MobileNavbar {
                constructor(mobileMenu, navList, navLinks) {
                    this.mobileMenu = document.querySelector(mobileMenu);
                    this.navList = document.querySelector(navList);
                    this.navLinks = document.querySelectorAll(navLinks);
                    this.activeClass = "active";
                    this.handleClick = this.handleClick.bind(this);
                }

                animateLinks() {
                    this.navLinks.forEach((link) => {
                        if(link.style.animation){
                            link.style.animation = '';
                        }
                        else{
                            link.style.animation = `navLinkFade 0.5s ease forwards 0.3s`;
                        }
                    });
                }

                handleClick() {
                    this.navList.classList.toggle(this.activeClass);
                    this.animateLinks();
                }
                addClickEvent() {
                    this.mobileMenu.addEventListener("click", this.handleClick);
                }
                init() {
                    if (this.mobileMenu) {
                        this.addClickEvent();
                    }
                    return this;
                }
            }
            const mobileNavbar = new MobileNavbar(
                ".mobile-menu",
                ".nav-list",
                ".nav-list li"
            );
            mobileNavbar.init();
        }
    }


}
</script>


<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Press+Start+2P&display=swap');

a {
    color: white;
    text-decoration: none;
    transition: 0.3s;
}

a:hover {
    opacity: 0.7;
}

.logo {
    font-size: 24px;
    text-transform: uppercase;
    letter-spacing: 4px;
    font-family: "Press Start 2P", system-ui;
}

nav {
    display: flex;
    justify-content: space-around;
    align-items: center;
    /* font-family: system-ui, -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, 'Open Sans', 'Helvetica Neue', sans-serif; */
    font-family: "Press Start 2P", system-ui;
    background-color: #23232e;
    height: 8vh;
    font-size: 13px;
}



.nav-list {
    list-style: none;
    display: flex;
}

.nav-list li {
    letter-spacing: 3px;
    margin-left: 32px;

}

.mobile-menu {
    display: none;
    cursor: pointer;
}

.mobile-menu div {
    width: 32px;
    height: 2px;
    background: #fff;
    margin: 8px;
}

@media (max-width: 999px) {


    .nav-list {
        position: absolute;
        opacity: 1;
        top: 8vh;
        right: 0;
        width: 50vw;
        height: 92vh;
        background-color: #23232e;
        flex-direction: column;
        align-items: center;
        justify-content: space-around;
        transform: translateX(100%);
        transition: transform 0.3s ease-in;
    }

    .nav-list li {
        margin-left: 0;
        opacity: 1;

    }

    .mobile-menu {

        display: block;
    }

    .nav-list.active {
        transform: translateX(0);
    }

    @keyframes navLinkFade {
        from {
            opacity: 0;
            transform: translateX(50px);
        }

        to {
            opacity: 1;
            transform: translateX(0);
        }
    }

}
</style>
