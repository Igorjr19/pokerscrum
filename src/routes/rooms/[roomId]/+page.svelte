<script lang="ts">
	import { page } from '$app/state';

	const roomId = page.params.roomId;

	let userName = '';
	let hasJoined = false;
	let isHost = false;
	let currentStory =
		'As a user, I want to be able to estimate story points so that the team can plan our sprint effectively.';
	let editingStory = false;
	let editedStoryText = '';
	let selectedEstimate: string | number | null = null;
	let submittedEstimate: string | number | null = null;
	let showResults = false;

	const urlParams = new URLSearchParams(window.location.search);
	const scaleType = urlParams.get('scale') || 'fibonacci';
	const forceVoting = urlParams.get('forceVoting') === 'true';

	const votingScales: Record<string, { name: string; values: Array<string | number> }> = {
		fibonacci: { name: 'Fibonacci', values: [0, 1, 2, 3, 5, 8, 13, 21, 34, '?'] },
		tshirt: { name: 'T-Shirt Sizes', values: ['XS', 'S', 'M', 'L', 'XL', 'XXL', '?'] },
		powers: { name: 'Powers of 2', values: [1, 2, 4, 8, 16, 32, 64, '?'] },
		linear: { name: 'Linear', values: [1, 2, 3, 4, 5, 6, 7, 8, 9, 10, '?'] },
		modified: { name: 'Modified Fibonacci', values: [0, 0.5, 1, 2, 3, 5, 8, 13, 20, 40, 100, '?'] }
	};

	$: currentScale = forceVoting
		? (votingScales[scaleType]?.values || votingScales.fibonacci.values).filter((v) => v !== '?')
		: votingScales[scaleType]?.values || votingScales.fibonacci.values;

	let participants: Array<{ name: string; estimate: string | number | null; hasVoted: boolean }> = [
		{ name: 'Alice', estimate: null, hasVoted: true },
		{ name: 'Bob', estimate: null, hasVoted: true },
		{ name: 'Charlie', estimate: null, hasVoted: true }
	];

	function joinRoom() {
		if (userName.trim()) {
			hasJoined = true;
			// ****** MOCK *******
			if (participants.length === 3) {
				isHost = true;
			}
			if (isHost) {
				participants = [...participants, { name: userName, estimate: null, hasVoted: false }];
			}
			// *** END OF MOCK ***
		}
	}

	function selectEstimate(value: string | number) {
		if (submittedEstimate === value) {
			submittedEstimate = null;
			selectedEstimate = null;
			updateUserVote(null, false);
		} else {
			selectedEstimate = value;
			submittedEstimate = value;
			updateUserVote(value, true);
		}
	}

	function updateUserVote(estimate: string | number | null, hasVoted: boolean) {
		participants = participants.map((p) =>
			p.name === userName ? { ...p, estimate, hasVoted } : p
		);
	}

	function revealResults() {
		if (!isHost) return;

		showResults = true;
		participants = participants.map((p) => {
			if (p.name === userName && isHost) {
				return p;
			}
			// ****** MOCK *******
			if (p.hasVoted) {
				return {
					...p,
					estimate: currentScale[Math.floor(Math.random() * (currentScale.length - 1))]
				};
			}
			// *** END OF MOCK ***
			return p;
		});
	}

	function nextStory() {
		if (!isHost) return;

		showResults = false;
		selectedEstimate = null;
		submittedEstimate = null;
		// ****** MOCK *******
		// participants = participants.map((p) => ({ ...p, estimate: null, hasVoted: false }));
		// *** END OF MOCK ***
	}

	function calculateAverage(): string {
		const numericEstimates = participants
			.map((p) => p.estimate)
			.filter((e) => e !== null && e !== '?' && typeof e === 'number') as number[];

		if (numericEstimates.length === 0) return '0';

		const sum = numericEstimates.reduce((a, b) => a + b, 0);
		return (sum / numericEstimates.length).toFixed(1);
	}

	function getMinEstimate(): string | number {
		const numericEstimates = participants
			.map((p) => p.estimate)
			.filter((e) => e !== null && e !== '?' && typeof e === 'number') as number[];

		return numericEstimates.length > 0 ? Math.min(...numericEstimates) : 0;
	}

	function getMaxEstimate(): string | number {
		const numericEstimates = participants
			.map((p) => p.estimate)
			.filter((e) => e !== null && e !== '?' && typeof e === 'number') as number[];

		return numericEstimates.length > 0 ? Math.max(...numericEstimates) : 0;
	}

	function getMostCommonEstimate(): string {
		const estimates = participants.map((p) => p.estimate).filter((e) => e !== null && e !== '?');

		if (estimates.length === 0) return 'None';

		const frequency: Record<string, number> = {};
		estimates.forEach((estimate) => {
			const key = String(estimate);
			frequency[key] = (frequency[key] || 0) + 1;
		});

		return Object.keys(frequency).reduce((a, b) => (frequency[a] > frequency[b] ? a : b));
	}

	function getUniqueEstimates(): string[] {
		return Array.from(
			new Set(
				participants
					.map((p) => p.estimate)
					.filter((e) => e !== null && e !== '?')
					.map((e) => String(e))
			)
		);
	}

	function allParticipantsSkipped(): boolean {
		const votedParticipants = participants.filter((p) => p.hasVoted && p.estimate !== null);
		return votedParticipants.length > 0 && votedParticipants.every((p) => p.estimate === '?');
	}

	function copyRoomUrl() {
		const roomUrl = window.location.href;
		navigator.clipboard
			.writeText(roomUrl)
			.then(() => {
                // TODO - Add a toast notification here
				console.log('Room URL copied to clipboard');
			})
			.catch((err) => {
				console.error('Failed to copy URL:', err);
			});
	}

	function startEditStory() {
		editedStoryText = currentStory;
		editingStory = true;
	}

	function updateStory() {
		if (editedStoryText.trim()) {
			currentStory = editedStoryText.trim();
			editingStory = false;
		}
	}

	function cancelEditStory() {
		editingStory = false;
		editedStoryText = '';
	}
</script>

<div class="container">
	{#if !hasJoined}
		<div class="join-screen">
			<h1>Join Room: {roomId}</h1>
			<form on:submit|preventDefault={joinRoom}>
				<input type="text" bind:value={userName} placeholder="Enter your name..." required />
				<button type="submit">Join Room</button>
			</form>
		</div>
	{:else}
		<div class="room">
			<header>
				<h1>Room: {roomId}</h1>
				<p>
					Welcome, {userName}! {#if isHost}<span class="host-badge">👑 Host</span>{/if}
				</p>
			</header>

			<section class="story">
				<div class="story-header">
					<h2>Current Story</h2>
					<div class="story-indicators">
						<span class="scale-indicator">Scale: {votingScales[scaleType]?.name || 'Unknown'}</span>
						{#if forceVoting}
							<span class="force-voting-indicator">🔒 Skip Disabled</span>
						{/if}
					</div>
				</div>
				{#if isHost && editingStory}
					<form on:submit|preventDefault={updateStory} class="story-edit">
						<textarea bind:value={editedStoryText} rows="3" placeholder="Enter story description..."
						></textarea>
						<div class="story-actions">
							<button type="submit">Save</button>
							<button type="button" on:click={cancelEditStory}>Cancel</button>
						</div>
					</form>
				{:else}
					<div class="story-display">
						<p class="story-text">{currentStory}</p>
						{#if isHost}
							<button class="edit-story-btn" on:click={startEditStory} title="Edit story">
								✏️ Edit
							</button>
						{/if}
					</div>
				{/if}
			</section>

			<section class="participants">
				<div class="participants-header">
					<h3>Participants ({participants.length})</h3>
					<button class="copy-url-btn" on:click={copyRoomUrl} title="Share room">
						🔗 Share Room
					</button>
				</div>
				<ul>
					{#each participants as participant}
						<li>
							<span class="participant-name">
								{participant.name}
								{#if participant.name === userName && isHost}
									<span class="participant-host-badge">👑 Host</span>
								{/if}
							</span>
							<span class="vote-status">
								{#if showResults && participant.estimate !== null}
									<strong>{participant.estimate}</strong>
								{:else if participant.hasVoted}
									✓ Voted
								{:else}
									Waiting...
								{/if}
							</span>
						</li>
					{/each}
				</ul>
			</section>

			<section class="estimation">
				<h3>Your Estimate</h3>
				<div class="cards">
					{#each currentScale as value (value)}
						<button
							class="card"
							class:selected={submittedEstimate === value}
							class:preview={selectedEstimate === value && submittedEstimate !== value}
							on:click={() => selectEstimate(value)}
							disabled={showResults}
						>
							{value}
							{#if submittedEstimate === value}
								<span class="voted-indicator">✓</span>
							{/if}
						</button>
					{/each}
				</div>
				<div class="vote-status-text">
					{#if submittedEstimate !== null && !showResults}
						<p class="voted">
							✅ You voted: <strong>{submittedEstimate}</strong> (click to change)
						</p>
					{:else if !showResults}
						<p class="not-voted">Select a card to vote</p>
					{/if}
				</div>

				{#if isHost}
					<div class="host-actions">
						{#if !showResults}
							<button
								on:click={revealResults}
								class="reveal-btn"
								disabled={participants.filter((p) => p.hasVoted).length === 0}
							>
								🎭 Reveal Results
							</button>
						{:else}
							<button on:click={nextStory} class="next-btn"> ➡️ Next Story </button>
						{/if}
					</div>
				{:else}
					<div class="waiting-host">
						{#if !showResults}
							<p>Waiting for host to reveal results...</p>
						{:else}
							<p>Waiting for host to start next story...</p>
						{/if}
					</div>
				{/if}
			</section>
			{#if showResults}
				<section class="results">
					<h3>Results</h3>
					{#if allParticipantsSkipped()}
						<div class="skip-message">
							<p>🤷‍♀️ Everyone skipped this story!</p>
							<p class="skip-subtitle">
								Consider breaking down the story or gathering more information before re-estimating.
							</p>
						</div>
					{:else}
						<div class="stats">
							{#if scaleType === 'tshirt'}
								<p>Most Common: {getMostCommonEstimate()}</p>
								<p>Estimates: {getUniqueEstimates().join(', ')}</p>
							{:else}
								<p>Average: {calculateAverage()}</p>
								<p>Min: {getMinEstimate()}</p>
								<p>Max: {getMaxEstimate()}</p>
							{/if}
						</div>
					{/if}
				</section>
			{/if}
		</div>
	{/if}
</div>

<style>
	.container {
		max-width: 1000px;
		margin: 0 auto;
		padding: 2rem;
	}

	.join-screen {
		text-align: center;
		margin-top: 5rem;
	}

	.join-screen h1 {
		margin-bottom: 2rem;
	}

	.join-screen form {
		display: flex;
		gap: 1rem;
		justify-content: center;
		max-width: 500px;
		margin: 0 auto;
	}

	header {
		margin-bottom: 2rem;
		border-bottom: 2px solid #eee;
		padding-bottom: 1rem;
	}

	header h1 {
		margin: 0;
		color: #333;
	}

	header p {
		margin: 0.5rem 0 0 0;
		color: #666;
	}

	.story {
		background-color: #f5f5f5;
		padding: 1.5rem;
		border-radius: 8px;
		margin-bottom: 2rem;
	}

	.story-header {
		display: flex;
		justify-content: space-between;
		align-items: center;
		margin-bottom: 1rem;
	}

	.story h2 {
		margin: 0;
		color: #333;
	}

	.story-indicators {
		display: flex;
		gap: 0.5rem;
		align-items: center;
		flex-wrap: wrap;
	}

	.scale-indicator {
		background-color: #2196f3;
		color: white;
		padding: 0.25rem 0.75rem;
		border-radius: 12px;
		font-size: 0.8rem;
		font-weight: bold;
	}

	.force-voting-indicator {
		background-color: #ff9800;
		color: white;
		padding: 0.25rem 0.75rem;
		border-radius: 12px;
		font-size: 0.8rem;
		font-weight: bold;
	}

	.story-text {
		font-size: 1.1rem;
		color: #555;
	}

	.participants ul {
		list-style: none;
		padding: 0;
	}

	.participants li {
		display: flex;
		justify-content: space-between;
		padding: 0.75rem;
		margin-bottom: 0.5rem;
		background-color: #f9f9f9;
		border-radius: 4px;
	}

	.vote-status {
		color: #4caf50;
		font-weight: 500;
	}

	.estimation {
		margin-top: 2rem;
	}

	.cards {
		display: flex;
		gap: 1rem;
		flex-wrap: wrap;
		margin: 1rem 0;
	}

	.card {
		width: 80px;
		height: 120px;
		background-color: #c3f3c3;
		border: 3px solid #ddd;
		border-radius: 8px;
		font-size: 2rem;
		font-weight: bold;
		cursor: pointer;
		transition: all 0.3s;
	}

	.card:hover:not(:disabled) {
		border-color: #4caf50;
		transform: translateY(-5px);
		box-shadow: 0 5px 15px rgba(0, 0, 0, 0.1);
	}

	.card.selected {
		border-color: #4caf50;
		background-color: #4caf50;
		color: white;
		position: relative;
	}

	.card.preview {
		border-color: #81c784;
		background-color: #e8f5e8;
	}

	.card:disabled {
		opacity: 0.5;
		cursor: not-allowed;
	}

	.voted-indicator {
		position: absolute;
		top: 4px;
		right: 4px;
		font-size: 1rem;
		color: white;
	}

	.host-badge {
		background-color: #ffd700;
		color: #333;
		padding: 0.25rem 0.5rem;
		border-radius: 12px;
		font-size: 0.8rem;
		font-weight: bold;
		margin-left: 0.5rem;
	}

	.vote-status-text {
		text-align: center;
		margin: 1rem 0;
	}

	.voted {
		color: #4caf50;
		font-weight: 600;
	}

	.not-voted {
		color: #666;
	}

	.host-actions {
		text-align: center;
		margin-top: 2rem;
	}

	.waiting-host {
		text-align: center;
		margin-top: 2rem;
		color: #888;
		font-style: italic;
	}

	.next-btn {
		background-color: #ff9800;
	}

	.next-btn:hover {
		background-color: #f57c00;
	}

	.host-actions button {
		margin: 0 0.5rem;
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

	button:hover:not(:disabled) {
		background-color: #45a049;
	}

	button:disabled {
		background-color: #ccc;
		cursor: not-allowed;
	}

	.reveal-btn {
		background-color: #2196f3;
	}

	.reveal-btn:hover {
		background-color: #0b7dda;
	}

	input[type='text'] {
		flex: 1;
		padding: 0.75rem;
		border: 2px solid #ddd;
		border-radius: 4px;
		font-size: 1rem;
	}

	input[type='text']:focus {
		outline: none;
		border-color: #4caf50;
	}

	.results {
		margin-top: 2rem;
		padding: 1.5rem;
		background-color: #e8f5e9;
		border-radius: 8px;
	}

	.stats {
		display: flex;
		gap: 2rem;
		font-size: 1.1rem;
	}

	.stats p {
		margin: 0;
	}

	.skip-message {
		text-align: center;
		padding-bottom: 2rem;
	}

	.skip-message p:first-child {
		font-size: 1.5rem;
		font-weight: bold;
		color: #ff9800;
		margin: 0 0 1rem 0;
	}

	.skip-subtitle {
		color: #666;
		font-style: italic;
		margin: 0;
	}

	.host-badge,
	.participant-host-badge {
		background-color: #fff9c4;
		color: #f57f17;
		border: 1px solid #ffb300;
		padding: 0.25rem 0.6rem;
		border-radius: 20px;
		font-size: 0.75rem;
		font-weight: bold;
		margin-left: 0.5rem;
		text-transform: uppercase;
		box-shadow: 0 2px 4px rgba(255, 193, 7, 0.2);
	}

	.participant-name {
		display: flex;
		align-items: center;
		gap: 0.5rem;
	}

	.participants-header {
		display: flex;
		justify-content: space-between;
		align-items: center;
		margin-bottom: 1rem;
	}

	.participants-header h3 {
		margin: 0;
	}

	.copy-url-btn {
		background-color: #2196f3;
		color: white;
		border: none;
		padding: 0.5rem 0.75rem;
		font-size: 0.8rem;
		border-radius: 4px;
		cursor: pointer;
		font-weight: bold;
		transition: background-color 0.2s ease;
	}

	.copy-url-btn:hover {
		background-color: #1976d2;
	}

	.participants ul li {
		display: flex;
		justify-content: space-between;
		align-items: center;
		padding: 0.75rem;
		border-bottom: 1px solid #eee;
	}

	.card {
		display: flex;
		flex-direction: column;
		justify-content: center;
		align-items: center;
		text-align: center;
		line-height: 1.2;
		font-family: 'Arial Black', 'Helvetica Bold', sans-serif;
		font-weight: 900;
		font-size: 1.5rem;
		letter-spacing: -0.5px;
	}

	.story-display {
		display: flex;
		justify-content: space-between;
		align-items: flex-start;
		gap: 1rem;
	}

	.story-text {
		flex: 1;
		margin: 0;
	}

	.edit-story-btn {
		background-color: #4caf50;
		color: white;
		border: none;
		padding: 0.5rem 0.75rem;
		font-size: 0.8rem;
		border-radius: 4px;
		cursor: pointer;
		white-space: nowrap;
		font-weight: bold;
		transition: background-color 0.2s ease;
	}

	.edit-story-btn:hover {
		background-color: #45a049;
	}

	.story-edit {
		margin-top: 1rem;
	}

	.story-edit textarea {
		width: 100%;
		padding: 0.75rem;
		border: 2px solid #ddd;
		border-radius: 4px;
		font-family: inherit;
		font-size: 1rem;
		resize: vertical;
		margin-bottom: 1rem;
	}

	.story-edit textarea:focus {
		outline: none;
		border-color: #4caf50;
	}

	.story-actions {
		display: flex;
		gap: 0.5rem;
	}

	.story-actions button {
		padding: 0.5rem 1rem;
		font-size: 0.9rem;
	}

	.story-actions button[type='button'] {
		background-color: #f5f5f5;
		color: #666;
	}

	.story-actions button[type='button']:hover {
		background-color: #e0e0e0;
	}
</style>
