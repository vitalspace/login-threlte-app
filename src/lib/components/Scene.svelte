<script lang="ts">
	import { T } from '@threlte/core';
	import { HTML, OrbitControls, Text, createTransition } from '@threlte/extras';
	import { spring } from 'svelte/motion';
	import type { Mesh } from 'three';
	import { cubicOut } from 'svelte/easing';
	import { Color, MeshStandardMaterial } from 'three';
	import { degToRad } from 'three/src/math/MathUtils';
	const getRandomColor = () => `#${Math.floor(Math.random() * 16777215).toString(16)}`;
	let material = new MeshStandardMaterial({
		color: new Color(getRandomColor())
	});
	const onClick = () => {
		material.color.set(getRandomColor());
		material = material;
	};
	let isHovering = false;
	let isPointerDown = false;
	let htmlPosZ = spring(0);
	$: htmlPosZ.set(isPointerDown ? -0.15 : isHovering ? 0 : 0.2, {
		hard: isPointerDown
	});

	const scale = createTransition<Mesh>((ref) => {
		return {
			tick(t: number) {
				// t = 0.5 + t * 0.5
				t = cubicOut(t);
				ref.scale.set(t, t, t);
			},
			// easing: cubicOut,
			duration: 400
		};
	});
</script>

<T.PerspectiveCamera position={[10, 5, 25]} makeDefault fov={30}>
	<OrbitControls
		enableDamping
		maxPolarAngle={degToRad(130)}
		minPolarAngle={degToRad(50)}
		maxAzimuthAngle={degToRad(30)}
		minAzimuthAngle={degToRad(-30)}
		enableZoom={false}
		enablePan={false}
	/>
</T.PerspectiveCamera>

<T.DirectionalLight position={[0, 10, 10]} />

<T.AmbientLight intensity={0.3} />

<!-- <T.GridHelper /> -->

<!-- <T.Mesh position.y={0.5} {material}>
	<T.SphereGeometry args={[0.5]} />
	<HTML position.y={1.25} position.z={$htmlPosZ} transform>
		<button
			on:pointerenter={() => (isHovering = true)}
			on:pointerleave={() => {
				isPointerDown = false;
				isHovering = false;
			}}
			on:pointerdown={() => (isPointerDown = true)}
			on:pointerup={() => (isPointerDown = false)}
			on:pointercancel={() => {
				isPointerDown = false;
				isHovering = false;
			}}
			on:click={onClick}
			class="bg-orange-500 rounded-full px-3 text-white hover:opacity-90 active:opacity-70"
		>
			Hi i'm regular Button
		</button>
	</HTML>

	<HTML position.x={0.75} transform pointerEvents="none">
		<p
			class="text-xs w-auto translate-x-1/2 drop-shadow-lg"
			style="color: #{material.color.getHexString()}"
		>
			color: #{material.color.getHexString()}
		</p>
	</HTML>

	<T.BoxGeometry args={[4, 2, 1]}  />

</T.Mesh> -->

<Text
	text="Login App"
	fontSize={1}
	color="white"
	position.y={3}
	anchorX="center"
	outlineWidth={0.2}
	outlineColor="#fe3d00"
/>

<T.Mesh position.y={0} position.z={0} >
	<T.BoxGeometry args={[5.5, 3, 0.5]} />
	<T.MeshBasicMaterial color="#5c5b5b" />

	<HTML position.z={0.2} position.y={0.4} transform PointerEvent="none" transition={scale}>
		<div class="flex flex-col gap-2">
			<input
				class="outline-none pl-2 rounded-sm w-48 text-[#9090a1]"
				type="emailss"
				placeholder="example@example.com"
			/>
			<input
				class="outline-none pl-2 rounded-sm w-48 text-[#9090a1]"
				type="password"
				placeholder="Password"
			/>
		</div>
	</HTML>

	<HTML position.z={$htmlPosZ} position.y={-0.8} transform PointerEvent="none">
		<div>
			<button
				on:pointerenter={() => (isHovering = true)}
				on:pointerleave={() => {
					isPointerDown = false;
					isHovering = false;
				}}
				on:pointerdown={() => (isPointerDown = true)}
				on:pointerup={() => (isPointerDown = false)}
				on:pointercancel={() => {
					isPointerDown = false;
					isHovering = false;
				}}
				on:click={onClick}
				class=" w-48 rounded-sm bg-green-400 text-white hover:bg-green-500 transition-all"
				>Submit</button
			>
		</div>
	</HTML>
</T.Mesh>
