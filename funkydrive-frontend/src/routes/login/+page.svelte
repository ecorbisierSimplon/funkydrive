<script lang="ts">
	import { setCookie } from 'typescript-cookie';
	import { onMount } from 'svelte';
	import type { ActionData, LoginData } from '$lib/packages/types';
	import { session } from '../session';
	import { goto } from '$app/navigation';

	export let data: LoginData;
	export let form: ActionData;
	onMount(() => {
		if (data.login) {
			let cookieOptions: { expires?: number; path: string } = { path: '/' };
			if (data.remember === 'on') {
				cookieOptions = { expires: 10, ...cookieOptions };
			}
			setCookie('session', data.sessionid, cookieOptions);
			setCookie('surname', data.surName, cookieOptions);
			setCookie('firstname', data.firstName, cookieOptions);
			session.set(true);
		}
	});
</script>

<svelte:head>
	<title>Login | Funkydrive</title>
	<meta name="description" content="Drive" />
</svelte:head>

<h2>Login</h2>
{#if $session}
	<div class="modals">
		<div class="">
			<h3>Yous is connected !</h3>
			<p>Welcome back, {data.firstName}</p>
			<button class="plus" on:click={() => goto('/')}>Ok</button>
		</div>
	</div>
{:else}
	<section>
		<form method="POST" action="?/login">
			<div class="mb-3 mt-3">
				<label class="form-label"
					>Email
					<input name="email" type="text" class="form-control" />
					<span class="error">{form?.emailError ?? ''}</span>
				</label>
			</div>
			<div class="mb-3">
				<label class="form-label"
					>Password
					<input name="password" type="password" class="form-control" />
					<span class="error">{form?.passwordError ?? ''}</span>
				</label>
			</div>
			<div class="error">
				{#if form?.errorEmail}
					<span>{form?.errorEmail}</span>
				{/if}
			</div>
			<div class="form-check mb-3">
				<label class="form-check-label">
					<input class="form-check-input" type="checkbox" name="remember" /> Remember me
				</label>
			</div>

			<button class="submit f--right"><i class="fas fa-user-alt"></i> Log in</button>
			<!-- <button formaction="?/register">Register</button> -->
		</form>
	</section>
{/if}

<style lang="scss">
	section {
		display: flex;
	}

	p {
		color: aliceblue;
	}
	label {
		width: 100%;
	}
	button {
		display: block;
		margin: 0 auto;
	}
</style>
