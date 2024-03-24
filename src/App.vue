<template>

    <main>
        <h1>Testing</h1>

        <p>Lorem ipsum dolor sit amet, consectetur adipisicing elit. Animi ipsum possimus quibusdam reprehenderit totam
            velit? Blanditiis consequuntur doloremque illo iste neque odio sapiente. Amet culpa iure nihil nobis saepe
            tempore?</p>
        <p>Est numquam quod unde! Aliquam at autem doloribus dolorum ea, esse illum in ipsam laboriosam molestiae nisi
            quae quod voluptates? Commodi cumque esse excepturi explicabo quasi qui quis unde, vitae.</p>
        <p>Accusamus expedita harum hic id in libero neque placeat recusandae tenetur voluptates. At corporis, cumque
            dicta eaque est facilis illum iste laboriosam molestiae molestias, quam qui rem repellat voluptas
            voluptate.</p>
        <p>Ab, accusamus atque aut corporis eligendi enim et facilis, harum hic, maxime quaerat quam sapiente tempora
            vitae voluptatem. Cum eius eveniet excepturi fuga itaque labore neque perspiciatis saepe, totam veniam?</p>

        <p>Lorem ipsum dolor sit amet, consectetur adipisicing elit. Deleniti in nesciunt omnis repellendus.
            Consectetur, recusandae totam? Accusamus ad eius quos rem soluta, ullam. Architecto facere illum magni
            provident reprehenderit voluptate.</p>
        <p>Aspernatur consectetur cum debitis est expedita inventore magnam molestiae odio, voluptatum? Accusamus ad
            amet architecto asperiores aut excepturi in, necessitatibus numquam odio odit placeat possimus quisquam
            tempore ullam unde voluptatem.</p>
        <p>Cupiditate deleniti dolor est odio odit quasi repudiandae tempore tenetur. A debitis, deserunt error et fugit
            id illum ipsa iusto libero molestias officia quia quibusdam rerum vero voluptatum? Necessitatibus, nisi!</p>
        <p>Architecto blanditiis ducimus laudantium modi obcaecati quam suscipit, voluptatem. Accusamus enim excepturi
            expedita illo laborum maiores, nam natus nihil obcaecati omnis quasi reiciendis sequi veniam? Accusamus
            asperiores exercitationem fugit nisi.</p>
    </main>

</template>

<script setup>

import {onBeforeUnmount, onMounted, onUnmounted} from "vue";
import * as THREE from 'three';

const scene = new THREE.Scene();
const camera = new THREE.PerspectiveCamera( 75, window.innerWidth / window.innerHeight, 0.1, 1000 );
let renderer = new THREE.WebGLRenderer({ alpha: true, antialias: true });
renderer.setPixelRatio( window.devicePixelRatio );

let geometry = new THREE.BoxGeometry( 1, 1, 1 );
// let material = new THREE.MeshBasicMaterial( { color: "cornflowerblue" } );
let material = new THREE.MeshPhongMaterial({color: 'cornflowerblue'});
let cube = new THREE.Mesh( geometry, material );

const directionalLight = new THREE.DirectionalLight(0xFFFFFF, 3);
directionalLight.position.set(1, 1, 10);
directionalLight.target.position.set(0, 0, 0);

const ambientLight = new THREE.AmbientLight(0xFFFFFF, 0.2);





window.addEventListener('resize', onResize, false); //When window is resized, call onResize() function.

const val = Math.random();

function onResize() {
    console.log("Resize", val);
    if (!camera) return;
    if (!renderer) return;

    camera.aspect = window.innerWidth / window.innerHeight; //Camera aspect ratio.
    camera.updateProjectionMatrix(); //Updating the display
    renderer.setSize(window.innerWidth, window.innerHeight) //Setting the renderer to the height and width of the window.
}

onMounted(() => {

    renderer.setSize( window.innerWidth, window.innerHeight );
    renderer.domElement.classList.add("bob");
    document.body.appendChild( renderer.domElement );

    scene.add( cube );
    scene.add( directionalLight );
    scene.add( directionalLight.target );
    scene.add( ambientLight );

    camera.position.z = 5;

    function animate() {
        if (!renderer) return;
        requestAnimationFrame( animate );

        cube.rotation.x += 0.01;
        cube.rotation.y += 0.01;

        renderer.render( scene, camera );
    }

    window.addEventListener("resize", onResize);
    animate();
})

onBeforeUnmount(() => {
    console.log("Unmounted", val);
    window.removeEventListener("resize", onResize);
    try {
        renderer.domElement.remove();
    } catch (e) {
        console.warn("Unable to remove renderer dom element", e);
    }

    try {
        renderer.dispose();
        geometry.dispose();
        material.dispose();
    } catch(e) {
        console.error("Unable to dispose of renderer", e);
    } finally {
        renderer = null;
    }

})



</script>



<style scoped>


</style>
