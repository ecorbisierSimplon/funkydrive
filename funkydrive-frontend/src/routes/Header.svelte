<script lang="ts">
	import { onMount } from 'svelte';
	import { session, sessionKey } from './session';
	import { getCookie } from 'typescript-cookie';
	import type { ActionData } from './$types';

	export let data: ActionData;
	let hostname: string = '';
	let logo: string = data.api + '/image/logo.png';

	let navCheck = false;

	function handleMenu() {
		if (navCheck) {
			navCheck = false;
			document.body.removeEventListener('click', handleMenu);
		} else {
			navCheck = true;
			document.body.addEventListener('click', handleMenu);
		}
	}

	onMount(() => {
		hostname = window.location.origin;
		if (getCookie('session')) {
			session.set(true);
			sessionKey.set(getCookie('session') || '');
		} else {
			session.set(false);
			sessionKey.set('');
		}
	});
</script>

<header>
	<div class="header">
		<div id="logo" class="logo">
			<a href={hostname}>
				<img src={logo} alt="" />
				<span class="text3d">FunkyDrive</span>
			</a>
		</div>
		<nav class="nav">
			<input
				type="checkbox"
				id="navicon-checkbox"
				class="navicon__checkbox"
				bind:checked={navCheck}
			/>
			<!-- <label for="navicon-checkbox" class="navicon__label"> -->
			<!-- for="navicon-checkbox" -->
			<button id="navicon__bt" class="navicon__label" on:click|stopPropagation={handleMenu}>
				<span class="navicon__box">
					<span class="navicon__hamburger"></span>
					<span class="navicon__hamburger"></span>
					<span class="navicon__hamburger"></span>
				</span>
				<!-- </label> -->
			</button>
			<div class="navbar">
				<ul>
					<li><a href="/"><i class="fa fa-home"></i>Home</a></li>
					<li><a href="/drive"><i class="fa fa-clipboard-check"></i>Choice file</a></li>
					{#if $session}
						<li>
							<a href="/create_drive"><i class="fa fa-edit"></i>Create Drive</a>
						</li>
					{/if}
					{#if !$session}
						<li><a href="/register"><i class="far fa-id-card"></i>Register</a></li>
						<li><a href="/login"><i class="fa fa-user-alt"></i>Login</a></li>
					{/if}
					{#if $session}
						<li>
							<a href="/logout"><i class="fa fa-user-alt-slash"></i>Logout</a>
						</li>
					{/if}
				</ul>
			</div>
		</nav>
	</div>
</header>

<style lang="scss">
	$color_p: 220;
	$color_p2: 230;

	$mobil-max: 748px;
	$pc-min: calc($mobil-max + 1px);
	$bg: hsl(26, 12%, 89%);

	$logo-size-desktop: 4;
	$logo-size-mobil: 2;
	$logo-size-delta: calc(1 + (calc(1 - calc(calc($logo-size-desktop / $logo-size-mobil) / 10))));

	header {
		margin: 10px;
		@media screen and (max-width: $mobil-max) {
			margin: 5px;
			margin-bottom: 15px;
		}
		.header {
			// display: flex;
			max-width: 1200px;
			padding: 15px;
			margin: 0 auto;
			@media screen and (max-width: $mobil-max) {
				padding: 5px;
			}
			.logo {
				margin: auto 20px;
				@media screen and (max-width: $mobil-max) {
					margin: auto 10px;
				}
				a {
					display: flex;
					flex-direction: row;
					flex-wrap: nowrap;
					align-items: center;
				}
				img {
					width: 100%;
					min-width: 60px;
					max-width: 100px;
					@media screen and (max-width: $mobil-max) {
						width: 80%;
						max-width: 70px;
					}
				}
				span {
					font-family: 'Madimi One', sans-serif;
					font-size: 4.5em;
					margin-left: 26px;
					@media screen and (max-width: $mobil-max) {
						font-size: 2rem;
						margin-left: 10px;
					}
				}
			}
		}
	}
	.nav {
		.navicon {
			&__checkbox {
				opacity: 0;
				position: absolute;
				left: -1000px;
				&:checked {
					& ~ .navicon__label .navicon__box span {
						background-color: hsl(0, 0%, 95%);

						&:first-child,
						&:last-child {
							background-color: hsl(0, 95%, 25%);
							border-bottom: 1px solid hsl(0, 0%, 0%);
							width: 35px;
							left: 1px;
						}

						&:first-child {
							transform: translate3d(6px, 10px, 0) rotate(45deg);
						}

						&:nth-child(2) {
							transform-origin: 0 0;
							transform: translate(-2000px);
							// rotate(-45deg) translate3d(-500px, 100px, 0);
							opacity: 0;
						}

						&:last-child {
							transform: translate3d(5px, -11px, 0) rotate(-45deg);
						}
					}
					& ~ .navbar {
						right: 0px;
						top: 0px;
						box-shadow: 2px 2px 9px 2px hsla(310, 5%, 23%, 0.637);
						width: 35vw;
						height: 100vh;
						background-color: hsl($color_p, 25%, 65%);
						padding: 8px;
						@media screen and (max-width: $mobil-max) {
							width: 260px;
						}
					}
				}
			}
			&__label {
				position: absolute;
				box-sizing: content-box;
				right: 15px;
				top: 12px;
				z-index: 999;
				margin: 6px 0 0 0;
				padding: 0 3px 0 2px;
				width: 40px;
				height: 40px;
				border-radius: 5px;
				display: inline-block;
				cursor: pointer;
				transition-property: opacity, filter;
				transition-duration: 0.25s;
				transition-timing-function: linear;
				font: inherit;
				color: inherit;
				text-transform: none;
				background-color: rgba(0, 0, 0, 0);
				border-width: 0 2px 2px 0;
				border-bottom: groove;
				border-right: groove;
				overflow: visible;
				background-color: hsl($color_p, 50%, 70%);
			}

			&__box {
				span {
					position: relative;
					display: block;
					top: 12px;
					margin-top: -4px;
					width: 30px;
					height: 5px;
					left: 8px;
					border-bottom: 3px solid #000;
					border-radius: 30%;
					position: absolute;
					transition-property: transform;
					transition-duration: 0.25s;
					transition-timing-function: ease;
					&:nth-child(2) {
						content: '';
						top: 22px;
						transition-property: transform, opacity;
						transition-timing-function: ease;
						transition-duration: 2.2s;
					}
					&:last-child {
						content: '';
						top: auto;
						bottom: 6px;
					}
				}
			}
		}
		.navbar {
			position: absolute;
			display: block;
			z-index: 998;
			right: 0px;
			top: 12px;
			overflow: hidden;
			transition: 0.5s all 0s;
			width: 4px;
			height: 50px;
			background-color: #9e0000;
			padding: 0px;
		}
		ul {
			margin: 0;
			list-style: none;
			position: relative;
			top: 90px;
			i {
				text-align: center;
				width: 30px;
				margin-right: 5px;
			}
		}
	}

	@function generate-text-shadow($shadow-size) {
		$shadow-size-delta: calc($shadow-size / 0.8);
		$hsl-base: hsl($color_p, 61%, 30%);
		$hsla-base: hsla($color_p, 50%, 0%, 0.9);

		$shadows: ();

		$shadows: append(
			$shadows,
			#{calc(-1px / #{$shadow-size}) calc(-1px / #{$shadow-size}) calc(1px / #{$shadow-size})
				hsl(50, 27%, 91%)},
			comma
		);

		@for $i from 1 through 15 {
			$shadows: append(
				$shadows,
				#{calc(0px / #{$shadow-size-delta})
					calc(#{$i}px / #{$shadow-size-delta})
					calc(0px / #{$shadow-size-delta})
					hsl($color_p, 61%, calc(calc(30 - $i) * 1%))},
				comma
			);
		}

		$shadows: append(
			$shadows,
			#{calc(2px / #{$shadow-size-delta}) calc(20px / #{$shadow-size-delta})
				calc(5px / #{$shadow-size-delta}) #{$hsla-base}},
			comma
		);
		$shadows: append(
			$shadows,
			#{calc(5px / #{$shadow-size-delta}) calc(23px / #{$shadow-size-delta})
				calc(5px / #{$shadow-size-delta}) hsla($color_p, 50%, 0%, 0.3)},
			comma
		);
		$shadows: append(
			$shadows,
			#{calc(8px / #{$shadow-size-delta}) calc(27px / #{$shadow-size-delta})
				calc(8px / #{$shadow-size-delta}) hsla($color_p, 50%, 0%, 0.5)},
			comma
		);
		$shadows: append(
			$shadows,
			#{calc(8px / #{$shadow-size-delta}) calc(28px / #{$shadow-size-delta})
				calc(15px / #{$shadow-size-delta}) hsla($color_p, 50%, 0%, 0.9)},
			comma
		);

		@return $shadows;
	}

	.text3d {
		color: hsl($color_p, 41%, 50%);
		letter-spacing: 2px;
		text-shadow: generate-text-shadow(calc($logo-size-desktop / $logo-size-desktop));

		@media screen and (max-width: $mobil-max) {
			text-shadow: generate-text-shadow($logo-size-delta);
		}
	}
</style>
