<template>
	<a-scene embedded>
		<a-assets>
        <img id="AE" crossorigin="anonymous" src="https://cardboard.cs.rpi.edu/2017_11_04/27.jpg">
        <img id="AE-thumb" crossorigin="anonymous" src="https://cdn.aframe.io/360-image-gallery-boilerplate/img/thumb-city.jpg">
        <img id="Sage-thumb" crossorigin="anonymous" src="https://cdn.aframe.io/360-image-gallery-boilerplate/img/thumb-cubes.jpg">
        <img id="Quad-thumb" crossorigin="anonymous" src="https://cdn.aframe.io/360-image-gallery-boilerplate/img/thumb-sechelt.jpg">
        <!--
        <audio id="click-sound" crossorigin="anonymous" src="https://cdn.aframe.io/360-image-gallery-boilerplate/audio/click.ogg"></audio>
        -->
        <img id="Sage" crossorigin="anonymous" src="https://cardboard.cs.rpi.edu/2017_11_04/2.jpg">
        <img id="Quad" crossorigin="anonymous" src="https://cardboard.cs.rpi.edu/2017_11_04/4.jpg">

        <!-- Image link template to be reused. -->
        <script id="link" type="text/html">
          <a-entity class="link"
            geometry="primitive: plane; height: 1; width: 1"
            material="shader: flat; src: ${thumb}"
            event-set__1="_event: mousedown; scale: 1 1 1"
            event-set__2="_event: mouseup; scale: 1.2 1.2 1"
            event-set__3="_event: mouseenter; scale: 1.2 1.2 1"
            event-set__4="_event: mouseleave; scale: 1 1 1"
            set-image="on: click; target: #image-360; src: ${src}"
            ></a-entity>
        </script>
      </a-assets>

      <!-- 360-degree image. -->
			<a-sky id="image-360" radius="10" src="#AE"></a-sky>

			<a-text value="Rensselaer\nPolytechnic\nInstitute\nin Virtual Reality" position="-1.5 2.5 -3"></a-text>

      <!-- Image links. -->
      <a-entity id="links" layout="type: line; margin: 1.5" position="0 -1 -4">
        <a-entity template="src: #link" data-src="#Sage" data-thumb="#Sage-thumb"></a-entity>
        <a-entity template="src: #link" data-src="#AE" data-thumb="#AE-thumb"></a-entity>
        <a-entity template="src: #link" data-src="#Quad" data-thumb="#Quad-thumb"></a-entity>
      </a-entity>

      <!-- Camera + cursor. -->
      <a-entity camera look-controls wasd-controls>
        <a-cursor id="cursor"
          animation__click="property: scale; startEvents: click; from: 0.1 0.1 0.1; to: 1 1 1; dur: 150"
          animation__fusing="property: fusing; startEvents: fusing; from: 1 1 1; to: 0.1 0.1 0.1; dur: 1500"
          event-set__1="_event: mouseenter; color: springgreen"
          event-set__2="_event: mouseleave; color: black"
          fuse="true"
          raycaster="objects: .link"></a-cursor>
				<a-entity geometry="primitive: plane; height: 0.2; width: 0.2" position="0 0 -1"
          material="color: gray; opacity: 0.5"></a-entity>
      </a-entity>
	</a-scene>
</template>

<style>

</style>

<script>
	console.log("hello from Home.vue")

	export default {
		name: 'Home',
		data() {
			return {
				// image: 'src/assets/nearempacequirectangular.jpg',
				image3: 'src/assets/near_sage.JPG'
			}
		}
	}
	AFRAME.registerComponent('set-image', {
	schema: {
		on: {type: 'string'},
		target: {type: 'selector'},
		src: {type: 'string'},
		dur: {type: 'number', default: 300}
	},

	init: function () {
		var data = this.data;
		var el = this.el;

		this.setupFadeAnimation();

		el.addEventListener(data.on, function () {
			// Fade out image.
			data.target.emit('set-image-fade');
			// Wait for fade to complete.
			setTimeout(function () {
				// Set image.
				data.target.setAttribute('material', 'src', data.src);
			}, data.dur);
		});
	},

	/**
	 * Setup fade-in + fade-out.
	 */
	setupFadeAnimation: function () {
		var data = this.data;
		var targetEl = this.data.target;

		// Only set up once.
		if (targetEl.dataset.setImageFadeSetup) { return; }
		targetEl.dataset.setImageFadeSetup = true;

		// Create animation.
		targetEl.setAttribute('animation__fade', {
			property: 'material.color',
			startEvents: 'set-image-fade',
			dir: 'alternate',
			dur: data.dur,
			from: '#FFF',
			to: '#000'
		});
	}
	});
</script>
