<template>
    <div class="container-fluid p-0">
        <Header />
        <slot />
    </div>
</template>

<style>
@font-face {
    font-family:"Kaisei Opti Bold" ;
    src: url('~/assets/font/KaiseiOpti-Bold.ttf');
}

body{
    font-family:"Kaisei Opti Regular" ;
}
</style>