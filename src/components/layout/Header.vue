<template>
    <section class="hero is-success">
        <div class="hero-head">
            <header class="navbar">
                <div class="container">
                    <div class="navbar-left">
                        <div class="navbar-item">
                            <strong>Platzi Music</strong>
                        </div>
                    </div>
                    <div class="navbar-right navbar-menu">

                    </div>
                </div>
            </header>
        </div>
        <div class="hero-body">
            <div class="container has-text-centered">
                <h1 class="title">Platzi Music</h1>
                <h2 class="subtitle">Besto canciones</h2>
                <PmPlayer></PmPlayer>
            </div>
        </div>
    </section>
</template>

<script>
import PmPlayer from '@/components/Player.vue'

export default {
    components: {
        PmPlayer
    }
}
</script>
