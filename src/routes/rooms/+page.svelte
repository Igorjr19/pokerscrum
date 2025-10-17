<script lang="ts">
	import { goto } from '$app/navigation';
	import { resolve } from '$app/paths';

	let roomName = '';
	let selectedScale: keyof typeof votingScales = 'fibonacci';
	let forceVoting = false;

	const votingScales = {
		fibonacci: { name: 'Fibonacci', values: [0, 1, 2, 3, 5, 8, 13, 21, 34, '?'] },
		tshirt: { name: 'T-Shirt Sizes', values: ['XS', 'S', 'M', 'L', 'XL', 'XXL', '?'] },
		powers: { name: 'Powers of 2', values: [1, 2, 4, 8, 16, 32, 64, '?'] },
		linear: { name: 'Linear', values: [1, 2, 3, 4, 5, 6, 7, 8, 9, 10, '?'] },
		modified: { name: 'Modified Fibonacci', values: [0, 0.5, 1, 2, 3, 5, 8, 13, 20, 40, 100, '?'] }
	};

	let rooms: Array<{ id: string; name: string; created: Date; scale: keyof typeof votingScales }> = [
		{ id: 'demo-room-1', name: 'Sprint 23 Planning', created: new Date(), scale: 'fibonacci' },
		{ id: 'demo-room-2', name: 'Feature Estimation', created: new Date(), scale: 'tshirt' }
	];

	function createRoom() {
		if (roomName.trim()) {
			const roomId = generateRoomId();
			const params = new URLSearchParams({ scale: selectedScale });
			if (forceVoting) {
				params.set('forceVoting', 'true');
			}
			goto(resolve(`/rooms/${roomId}?${params.toString()}`), { replaceState: false });
		}
	}

	function generateRoomId(): string {
		return Math.random().toString(36).substring(2, 15);
	}

	function joinRoom(roomId: string) {
		goto(resolve(`/rooms/${roomId}`), { replaceState: false });
	}
</script>

<div class="container">
	<h1>PokerScrum Rooms</h1>

	<section class="create-room">
		<h2>Create a New Room</h2>
		<form on:submit|preventDefault={createRoom}>
			<div class="form-group">
				<input type="text" bind:value={roomName} placeholder="Enter room name..." required />
			</div>

			<div class="form-group">
				<label for="scale-select">Voting Scale:</label>
				<select id="scale-select" bind:value={selectedScale}>
					{#each Object.entries(votingScales) as [key, scale]}
						<option value={key}>{scale.name}</option>
					{/each}
				</select>
				<div class="scale-preview">
					<strong>Cards:</strong>
					{votingScales[selectedScale].values.join(', ')}
				</div>
			</div>

			<div class="form-group">
				<label class="checkbox-label">
					<input type="checkbox" bind:checked={forceVoting} />
					Force Voting (Remove Skip Option)
				</label>
				<div class="option-description">
					When enabled, participants must vote and cannot skip stories
				</div>
			</div>

			<button type="submit">Create Room</button>
		</form>

		<div class="sharing-options">
			<h3>💡 Real-time Collaboration:</h3>
			<ul>
				<li><strong>P2P WebRTC:</strong> Direct peer-to-peer connections for real-time sync</li>
				<li><strong>Share the URL:</strong> Team members connect instantly via the room URL</li>
				<li><strong>No Backend Required:</strong> Everything works directly in your browser</li>
				<li><strong>Secure:</strong> All communication is encrypted between participants</li>
			</ul>
			<p class="tech-note">
				<strong>Note:</strong> WebRTC enables real-time estimation sharing without any server. The first
				person to join becomes the facilitator.
			</p>
		</div>
	</section>
	<section class="existing-rooms">
		<h2>Available Rooms</h2>
		{#if rooms.length > 0}
			<ul class="rooms-list">
				{#each rooms as room (room.id)}
					<li>
						<div class="room-info">
							<h3>{room.name}</h3>
							<p class="room-id">ID: {room.id}</p>
							<p class="room-scale">Scale: {votingScales[room.scale]?.name || 'Unknown'}</p>
						</div>
						<button on:click={() => joinRoom(room.id)}>Join Room</button>
					</li>
				{/each}
			</ul>
		{:else}
			<p class="no-rooms">No rooms available. Create one to get started!</p>
		{/if}
	</section>
</div>

<style>
	.container {
		max-width: 800px;
		margin: 0 auto;
		padding: 2rem;
	}

	h1 {
		color: #333;
		margin-bottom: 2rem;
	}

	.create-room,
	.existing-rooms {
		margin-bottom: 3rem;
	}

	h2 {
		color: #555;
		margin-bottom: 1rem;
		font-size: 1.5rem;
	}

	form {
		display: flex;
		flex-direction: column;
		gap: 1.5rem;
		margin-bottom: 2rem;
	}

	.form-group {
		display: flex;
		flex-direction: column;
		gap: 0.5rem;
	}

	input[type='text'] {
		padding: 0.75rem;
		border: 2px solid #ddd;
		border-radius: 4px;
		font-size: 1rem;
	}

	label {
		font-weight: 600;
		color: #333;
	}

	select {
		padding: 0.75rem;
		border: 2px solid #ddd;
		border-radius: 4px;
		font-size: 1rem;
		background-color: white;
	}

	.scale-preview {
		margin-top: 0.5rem;
		padding: 0.5rem;
		background-color: #f5f5f5;
		border-radius: 4px;
		font-size: 0.9rem;
		color: #666;
	}

	.checkbox-label {
		display: flex;
		align-items: center;
		gap: 0.5rem;
		font-weight: 500;
		cursor: pointer;
	}

	.checkbox-label input[type="checkbox"] {
		width: auto;
		margin: 0;
		cursor: pointer;
	}

	.option-description {
		margin-top: 0.25rem;
		font-size: 0.85rem;
		color: #666;
		font-style: italic;
	}

	input[type='text']:focus {
		outline: none;
		border-color: #4caf50;
	}

	button {
		padding: 0.75rem 1.5rem;
		background-color: #4caf50;
		color: white;
		border: none;
		border-radius: 4px;
		font-size: 1rem;
		cursor: pointer;
		transition: background-color 0.3s;
	}

	button:hover {
		background-color: #45a049;
	}

	.rooms-list {
		list-style: none;
		padding: 0;
	}

	.rooms-list li {
		display: flex;
		justify-content: space-between;
		align-items: center;
		padding: 1rem;
		margin-bottom: 1rem;
		background-color: #f9f9f9;
		border: 1px solid #ddd;
		border-radius: 4px;
	}

	.room-info h3 {
		margin: 0 0 0.5rem 0;
		color: #333;
	}

	.room-id {
		margin: 0.25rem 0 0 0;
		color: #888;
		font-size: 0.875rem;
	}

	.room-scale {
		margin: 0.25rem 0 0 0;
		color: #666;
		font-size: 0.875rem;
		font-weight: 500;
	}

	.no-rooms {
		color: #888;
		text-align: center;
		padding: 2rem;
	}

	.sharing-options {
		margin-top: 2rem;
		padding: 1.5rem;
		background-color: #f0f8ff;
		border-radius: 8px;
		border-left: 4px solid #2196f3;
	}

	.sharing-options h3 {
		margin: 0 0 1rem 0;
		color: #2196f3;
		font-size: 1.1rem;
	}

	.sharing-options ul {
		margin: 0;
		padding-left: 1.5rem;
	}

	.sharing-options li {
		margin-bottom: 0.5rem;
		color: #555;
		line-height: 1.5;
	}
</style>
