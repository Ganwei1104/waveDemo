<template>
  <div>
    <div id="indexLizi"></div>
  </div>
</template>

<script>
    import * as THREE from 'three'
    export default {
        name: "index",
        data(){
          return{
            count:0,
            //用来跟踪鼠标水平位置
            mouseX:0,
            windowHalfX: null,
            //相机
            camera: null,
            //场景
            scene: null,
            //批量管理粒子
            particles: null,
            //渲染器
            renderer: null
          }
        },
        created(){

        },
        mounted(){
          this.init();
          this.animate();
        },
        methods:{
          init(){
            const SEPARATION = 100;
            //水平方向粒子数量
            const AMOUNTX = 50;
            //垂直方向粒子数量
            const AMOUNTY = 50;
            const SCREEN_WIDTH = window.innerWidth;
            const SCREEN_HRIGHT = window.innerHeight;
            // const container = document.createElement( 'div' );
            const container = document.getElementById('indexLizi');
            this.windowHalfX = window.innerWidth / 2;
            const top = 300;
            container.style.position = 'relative';
            container.style.top = `${top}px`;
            container.style.height = `${(SCREEN_HRIGHT - top)}px`;
            // document.getElementById("indexLizi").appendChild(container);

            this.camera = new THREE.PerspectiveCamera( 75, SCREEN_WIDTH / SCREEN_HRIGHT, 1, 10000 );
            this.camera.position.z = 1000;//1000
            this.scene = new THREE.Scene();
            const numParticles = AMOUNTX * AMOUNTY;

            const positions = new Float32Array( numParticles * 3 );
            const scales = new Float32Array( numParticles );

            //初始化粒子位置和大小
            let i = 0;
            let j = 0;
            for ( let ix = 0; ix < AMOUNTX; ix ++ ) {
              for ( let iy = 0; iy < AMOUNTY; iy ++ ) {
                positions[ i ] = ix * SEPARATION - ( ( AMOUNTX * SEPARATION ) / 2 ); // x
                positions[ i + 1 ] = 0; // y
                positions[ i + 2 ] = iy * SEPARATION - ( ( AMOUNTY * SEPARATION ) / 2 ); // z
                scales[ j ] = 1;
                i += 3;
                j ++;
              }
            }
            const geometry = new THREE.BufferGeometry();
            geometry.addAttribute( 'position', new THREE.BufferAttribute( positions, 3 ) );
            geometry.addAttribute( 'scale', new THREE.BufferAttribute( scales, 1 ) );

            const material = new THREE.ShaderMaterial( {
              uniforms: {
                color: { value: new THREE.Color( 0x097bdb ) },
              },
              // vertexShader: document.getElementById( 'vertexshader' ).textContent,
              // fragmentShader: document.getElementById( 'fragmentshader' ).textContent
              vertexShader:`
                attribute float scale;
                void main() {
                  vec4 mvPosition = modelViewMatrix * vec4( position, 1.0 );
                  gl_PointSize = scale * ( 300.0 / - mvPosition.z );
                  gl_Position = projectionMatrix * mvPosition;
                }
              `,
              fragmentShader:`
                uniform vec3 color;
                void main() {
                  if ( length( gl_PointCoord - vec2( 0.5, 0.5 ) ) > 0.475 ) discard;
                  gl_FragColor = vec4( color, 1.0 );
                }
              `
            } );

            //

            this.particles = new THREE.Points( geometry, material );
            this.scene.add( this.particles );

            //

            this.renderer = new THREE.WebGLRenderer( { antialias: true, alpha:true } );
            this.renderer.setSize( container.clientWidth, container.clientHeight );
            this.renderer.setPixelRatio( window.devicePixelRatio );
            this.renderer.setClearAlpha(0);
            container.appendChild( this.renderer.domElement );

            window.addEventListener( 'resize', this.onWindowResize, false );
            document.addEventListener( 'mousemove', this.onDocumentMouseMove, false );
            document.addEventListener( 'touchstart', this.onDocumentTouchStart, false );
            document.addEventListener( 'touchmove', this.onDocumentTouchMove, false );

          },

          render() {
            const AMOUNTX = 50;
            const AMOUNTY = 50;
            this.camera.position.x += ( this.mouseX - this.camera.position.x ) * .05;
            // this.camera.position.y += ( - this.mouseY - this.camera.position.y ) * .05;
            this.camera.position.y = 364;
            this.camera.lookAt( this.scene.position );
            const positions = this.particles.geometry.attributes.position.array;
            const scales = this.particles.geometry.attributes.scale.array;

            let i = 0;
            let j = 0;

            for ( let ix = 0; ix < AMOUNTX; ix ++ ) {

              for ( let iy = 0; iy < AMOUNTY; iy ++ ) {

                positions[ i + 1 ] = ( Math.sin( ( ix + this.count ) * 0.3 ) * 50 ) +
                  ( Math.sin( ( iy + this.count ) * 0.5 ) * 50 );

                scales[ j ] = ( Math.sin( ( ix + this.count ) * 0.3 ) + 1 ) * 8 +
                  ( Math.sin( ( iy + this.count ) * 0.5 ) + 1 ) * 8;

                i += 3;
                j ++;

              }

            }
            //重新渲染粒子
            this.particles.geometry.attributes.position.needsUpdate = true;
            this.particles.geometry.attributes.scale.needsUpdate = true;
            this.renderer.render( this.scene, this.camera );
            this.count += 0.1;
          },

          animate() {
            requestAnimationFrame( this.animate );
            this.render();
          },

          onWindowResize() {

            this.windowHalfX = window.innerWidth / 2;
            // this.windowHalfY = window.innerHeight / 2;
            this.camera.aspect = window.innerWidth / window.innerHeight;
            this.camera.updateProjectionMatrix();
            this.renderer.setSize( window.innerWidth, window.innerHeight );

        },

        //

          onDocumentMouseMove( event ) {

            this.mouseX = event.clientX - this.windowHalfX;
            // mouseY = event.clientY - windowHalfY;

          },
          onDocumentTouchStart( event ) {
            if ( event.touches.length === 1 ) {
              event.preventDefault();
              this.mouseX = event.touches[ 0 ].pageX - this.windowHalfX;
              // mouseY = event.touches[ 0 ].pageY - windowHalfY;
            }
          },

          onDocumentTouchMove( event ) {
            if ( event.touches.length === 1 ) {
              event.preventDefault();
              this.mouseX = event.touches[ 0 ].pageX - this.windowHalfX;
              // mouseY = event.touches[ 0 ].pageY - windowHalfY;
            }
          }
    }
    }
</script>

<style scoped>
  #indexLizi{
    /*position: fixed;*/
    height: 400px;
    /*bottom:0;*/
  }
</style>
