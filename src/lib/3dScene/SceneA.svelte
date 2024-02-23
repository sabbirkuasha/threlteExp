<script>
	import { T, useThrelte } from '@threlte/core';
	import { Environment, OrbitControls, TransformControls, SoftShadows } from '@threlte/extras';
	import { Portal } from '@threlte/extras';

	const { scene } = useThrelte();

	let helperWhiteLight;
	$: console.log(helperWhiteLight);
</script>

<Environment path="/hdr/" files="stadium_01_2k.hdr" strength={0.5} />

<T.PerspectiveCamera makeDefault position={[3, 3, 10]} lookAt.y={0.0} fov={30}>
	<OrbitControls enableDamping />
</T.PerspectiveCamera>

<T.GridHelper args={[10, 10]} />

<!-- Sphere -->
<T.Mesh position={[0, 0.5, 0]} scale={1} castShadow receiveShadow>
	<T.SphereGeometry castShadow recieveShadow />
	<T.MeshStandardMaterial roughness={1} color="gray" side={0} />
</T.Mesh>

<!-- Ground Plane -->
<T.Mesh position={[0, 0, 0]} rotation={[-1.5705, 0, 0]} castShadow receiveShadow scale={10}>
	<T.PlaneGeometry castShadow recieveShadow />
	<T.MeshStandardMaterial roughness={1} color="gray" side={0} />
</T.Mesh>

<SoftShadows focus={0} size={80} samples={40} />

<T.DirectionalLight
	let:ref
	castShadow
	shadow.mapSize.width={1024}
	shadow.mapSize.height={1024}
	shadow.bias={0.0001}
	intensity={2}
	position={[-4, 4.5, 0]}
>
	<TransformControls
		object={ref}
		on:objectChange={() => {
			console.log(ref);
			if (!helperWhiteLight) return;
			helperWhiteLight.update();
		}}
	/>
	<Portal object={scene}>
		<T.DirectionalLightHelper args={[ref]} bind:ref={helperWhiteLight} />
	</Portal>
</T.DirectionalLight>
