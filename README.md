# CG-project
Projeto da disciplina de Computação grafica dos alunos Pedro Pires e Renato Correa

# Documentation
  three.js é uma biblioteca para Javascript que procura simplificar o webGL.
# Explicação do código
  ```js

<script>
    //Aqui se faz a declaração de váriaveis que serão necessárias em outras funçoes fora de init()
    var camera, scene, renderer;
    var plane;
    var container;
    var obj;

    init(); //A função init roda uma vez e inicializa tudo o que será necessario para que o mundo seja criado
    render();// E a função render é um loop infinito que roda uma vez por frame

    function init(){
      container = document.createElement('div');
      document.body.appendChild(container);

      //Camera
      //Aqui criamos e inicializamos a posição da camera. Nesse caso estamos criando uma camera com perspectiva
      //PerspectiveCamera( fov, aspect, near, far )
      //
      camera = new THREE.PerspectiveCamera( 75, window.innerWidth / window.innerHeight, 0.1, 1000 );
      camera.position.set(0, -7, 3);

      //Scene

      scene = new THREE.Scene();

      //Renderer
      renderer = new THREE.WebGLRenderer();
      renderer.setPixelRatio(window.devicePixelRatio);
      renderer.setSize( window.innerWidth, window.innerHeight );
      container.appendChild( renderer.domElement );

      //Plane for the ground
      var planeGeometry = new THREE.PlaneBufferGeometry(20, 20, 20);
      var planeMaterial = new THREE.MeshBasicMaterial({color: 0x0f0f0f});
      plane = new THREE.Mesh(planeGeometry, planeMaterial);
      plane.position.set(0,0,0);
      scene.add(plane);

      //ambient light
      var ambient = new THREE.AmbientLight(0x101030);
      scene.add( ambient );

      //Spotlight
      var spotlight = new THREE.SpotLight( 0xffffff);
      spotlight.position.set(100, -100, 100);
      spotlight.castShadow = true;
      spotlight.shadow.mapSize.width = 1024;
      spotlight.shadow.mapSize.height = 1024;
      spotlight.shadow.camera.near = 500;
      spotlight.shadow.camera.far = 4000;
      spotlight.shadowCameraFOV = 30;
      scene.add ( spotlight );

      //obj file
      var loader = new THREE.OBJLoader(); 
      loader.load(
        '3DModels/Stormtrooper/Stormtrooper.obj',
        function ( object ) {
          object.scale.set(1, 1, 1);
          object.position.set(0, 0, 0);
          object.rotation.x = -80;
          scene.add( object );
          obj = object;
          });

      //Camera look at
      camera.lookAt(plane.position);

      window.addEventListener( 'resize', onWindowResize, false );

          }

    function onWindowResize() {

      camera.aspect = window.innerWidth / window.innerHeight;
      camera.updateProjectionMatrix();

      renderer.setSize( window.innerWidth, window.innerHeight );

      }

    function render() {

      requestAnimationFrame( render );
      obj.rotation.y += 0.03;
      renderer.render( scene, camera );
    }
</script>
  ```
