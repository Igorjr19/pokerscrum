<script lang="ts">
	import { resolve } from '$app/paths';
</script>

<section>
	<h1>About PokerScrum</h1>
</section>

<section>
	<h2>What is Planning Poker?</h2>
	<p>
		Planning Poker, also called Scrum Poker, is a consensus-based, gamified technique for
		estimating, mostly used to estimate effort or relative size of development goals in software
		development. In Planning Poker, members of the group make estimates by playing numbered cards
		face-down to the table, instead of speaking them aloud.
	</p>
</section>

<section>
	<h2>How PokerScrum Works</h2>
	<ol>
		<li><strong>Create a Room:</strong> The facilitator creates a new estimation room.</li>
		<li><strong>Invite Team Members:</strong> Share the room ID with your team.</li>
		<li>
			<strong>Present User Story:</strong> The team discusses the user story or task to be estimated.
		</li>
		<li><strong>Private Voting:</strong> Each team member selects their estimate privately.</li>
		<li>
			<strong>Reveal Results:</strong> Once everyone has voted, estimates are revealed simultaneously.
		</li>
		<li><strong>Discuss:</strong> Team discusses any significant differences in estimates.</li>
		<li><strong>Reach Consensus:</strong> Re-vote if needed until the team reaches agreement.</li>
	</ol>
</section>

<section>
	<h2>Benefits of Using PokerScrum</h2>
	<ul>
		<li><strong>Reduces Bias:</strong> Private voting prevents anchoring and groupthink.</li>
		<li>
			<strong>Encourages Discussion:</strong> Divergent estimates spark valuable conversations.
		</li>
		<li><strong>Engages the Team:</strong> Everyone participates in the estimation process.</li>
		<li><strong>Improves Accuracy:</strong> Collective intelligence leads to better estimates.</li>
		<li><strong>Remote-Friendly:</strong> Perfect for distributed teams.</li>
	</ul>
</section>

<section>
	<h2>The Fibonacci Sequence</h2>
	<p>
		PokerScrum uses the Fibonacci sequence (0, 1, 2, 3, 5, 8, 13, 21, 34) for story point
		estimation. This sequence reflects the inherent uncertainty in estimating larger items. The
		larger the estimate, the more uncertainty there is, which is reflected in the larger gaps
		between numbers.
	</p>
</section>

<section>
	<h2>Get Started</h2>
	<p>
		Ready to improve your team's estimation process? <a
			href={resolve('/rooms')}
			data-sveltekit-preload-data="hover">Create a room</a
		> and start estimating with PokerScrum today!
	</p>
</section>

<style>
	h1 {
		color: #333;
		margin-bottom: 2rem;
	}

	section {
		max-width: 800px;
		margin: 0 auto 3rem auto;
		padding: 0 2rem;
	}

	h2 {
		color: #555;
		margin-top: 2rem;
		margin-bottom: 1rem;
	}

	p {
		line-height: 1.6;
		color: #666;
	}

	ol,
	ul {
		line-height: 1.8;
		color: #666;
	}

	li {
		margin-bottom: 0.5rem;
	}

	strong {
		color: #333;
	}

	a {
		color: #4caf50;
		text-decoration: none;
	}

	a:hover {
		text-decoration: underline;
	}
</style>
