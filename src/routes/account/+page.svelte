<!-- src/routes/account/+page.svelte -->
<script>
	import { enhance } from '$app/forms'

	export let data
	export let form

	let { session, supabase, profile } = data
	$: ({ session, supabase, profile } = data)

	let profileForm
	let loading = false
	let fullName = profile?.full_name ?? ''
	let username = profile?.username ?? ''
	let website = profile?.website ?? ''
	let avatarUrl = profile?.avatar_url ?? ''

	const handleSubmit = () => {
		loading = true
		return async () => {
			loading = false
		}
	}

	const handleSignOut = () => {
		loading = true
		return async ({ update }) => {
			loading = false
			update()
		}
	}
</script>

<div class="form-widget">
	<form
		class="form-widget"
		method="post"
		action="?/update"
		use:enhance={handleSubmit}
		bind:this={profileForm}
	>
		<div>
			<label for="email">Email</label>
			<input id="email" type="text" value={session.user.email} disabled />
		</div>

		<div>
			<label for="fullName">Full Name</label>
			<input id="fullName" name="fullName" type="text" value={form?.fullName ?? fullName} />
		</div>

		<div>
			<label for="username">Username</label>
			<input id="username" name="username" type="text" value={form?.username ?? username} />
		</div>

		<div>
			<label for="website">Website</label>
			<input id="website" name="website" type="url" value={form?.website ?? website} />
		</div>

		<div>
			<input
				type="submit"
				class="button block primary"
				value={loading ? 'Loading...' : 'Update'}
				disabled={loading}
			/>
		</div>
	</form>

	<form method="post" action="?/signout" use:enhance={handleSignOut}>
		<div>
			<button class="button block" disabled={loading}>Sign Out</button>
		</div>
	</form>
</div>
