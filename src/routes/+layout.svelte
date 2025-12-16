<script>
	import checklist from '$lib/assets/check-lists-square-svgrepo-com.svg';

	let { children } = $props();
</script>

<svelte:head>
	<link rel="icon" href={checklist} />
	<link rel="preconnect" href="https://fonts.googleapis.com">
	<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
	<link href="https://fonts.googleapis.com/css2?family=Karla:ital,wght@0,200..800;1,200..800&family=Spectral:ital,wght@0,200;0,300;0,400;0,500;0,600;0,700;0,800;1,200;1,300;1,400;1,500;1,600;1,700;1,800&display=swap" rel="stylesheet">
</svelte:head>

<style>
	:global(body) {
		font-family: "Spectral", serif;
		font-weight: 400;
  		font-style: normal;
		margin: 10vw 35vw 10vw 35vw;
		background-image: linear-gradient(#bde9e7, #6DB1BB);
	}
	:global(p) {
		font-family: "Karla", sans-serif;
		font-weight: 400;
		font-style: normal;
		font-size: 22px;
		margin-top: 0;
		color: #048da2
	}
	:global(h1) {
        font-size: 55px;
		margin-bottom: 0;
    }
</style>

{@render children()}
