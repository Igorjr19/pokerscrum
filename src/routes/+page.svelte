<script lang="ts">
	import { resolve } from '$app/paths';
</script>

<svelte:head>
	<title>PokerScrum - Online Planning Poker for Agile Teams</title>
</svelte:head>

<div class="hero">
	<div class="hero-content">
		<h1>Welcome to PokerScrum</h1>
		<p class="subtitle">An online app to estimate story points for your agile projects</p>
		<div class="cta">
			<a href={resolve('/rooms')} class="btn-primary" data-sveltekit-preload-data="hover"
				>Get Started</a
			>
			<a href={resolve('/about')} class="btn-secondary" data-sveltekit-preload-data="hover"
				>Learn More</a
			>
		</div>
	</div>
</div>

<div class="container">
	<section class="intro">
		<h2>Collaborative Story Point Estimation Made Easy</h2>
		<p>
			PokerScrum is a simple and intuitive tool designed to help agile teams estimate story points
			collaboratively using the Planning Poker technique. Whether you're working remotely or in the
			same office, PokerScrum makes it easy to reach consensus on story point estimates.
		</p>
	</section>

	<section class="how-it-works">
		<h2>How It Works</h2>
		<div class="steps">
			<div class="step">
				<div class="step-number">1</div>
				<h3>Create a Room</h3>
				<p>Set up an estimation session for your team with a custom room name.</p>
			</div>
			<div class="step">
				<div class="step-number">2</div>
				<h3>Invite Your Team</h3>
				<p>Share the room ID with team members to join the estimation session.</p>
			</div>
			<div class="step">
				<div class="step-number">3</div>
				<h3>Vote Privately</h3>
				<p>Each participant selects their estimate privately using story point cards.</p>
			</div>
			<div class="step">
				<div class="step-number">4</div>
				<h3>Reveal & Discuss</h3>
				<p>Results are revealed simultaneously, encouraging discussion and consensus.</p>
			</div>
		</div>
	</section>

	<section class="features">
		<h2>Key Features</h2>
		<div class="feature-grid">
			<div class="feature">
				<div class="feature-icon">🏠</div>
				<h3>Easy Room Management</h3>
				<p>Create and manage estimation rooms with simple, shareable IDs.</p>
			</div>
			<div class="feature">
				<div class="feature-icon">🔒</div>
				<h3>Private Voting</h3>
				<p>Prevent bias with private voting that reveals results simultaneously.</p>
			</div>
			<div class="feature">
				<div class="feature-icon">⚡</div>
				<h3>Real-time Results</h3>
				<p>See voting progress and results updated in real-time across all devices.</p>
			</div>
			<div class="feature">
				<div class="feature-icon">📊</div>
				<h3>Many Estimation Scales</h3>
				<p>Use various estimation scales, including the proven Fibonacci sequence, for accurate story point estimation.</p>
			</div>
			<div class="feature">
				<div class="feature-icon">💬</div>
				<h3>Discussion Tools</h3>
				<p>Built-in features to facilitate productive estimation discussions.</p>
			</div>
			<div class="feature">
				<div class="feature-icon">🌐</div>
				<h3>Remote-Friendly</h3>
				<p>Perfect for distributed teams working from anywhere in the world.</p>
			</div>
		</div>
	</section>

	<section class="cta-section">
		<h2>Ready to Improve Your Estimation Process?</h2>
		<p>
			Whether you're a seasoned agile practitioner or new to story point estimation, PokerScrum
			provides a user-friendly platform to enhance your team's estimation accuracy.
		</p>
		<a href={resolve('/rooms')} class="btn-primary large" data-sveltekit-preload-data="hover"
			>Create or Join a Room Now</a
		>
	</section>
</div>

<style>
	.hero {
		background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
		color: white;
		padding: 6rem 2rem;
		text-align: center;
		margin-bottom: 4rem;
	}

	.hero-content h1 {
		font-size: 3.5rem;
		margin: 0 0 1rem 0;
		font-weight: 700;
	}

	.subtitle {
		font-size: 1.25rem;
		margin: 0 0 3rem 0;
		opacity: 0.9;
		max-width: 600px;
		margin-left: auto;
		margin-right: auto;
	}

	.cta {
		display: flex;
		gap: 1rem;
		justify-content: center;
		flex-wrap: wrap;
	}

	.btn-primary {
		background-color: #4caf50;
		color: white;
		padding: 1rem 2rem;
		text-decoration: none;
		border-radius: 8px;
		font-weight: 600;
		font-size: 1.1rem;
		transition: all 0.3s;
		display: inline-block;
	}

	.btn-primary:hover {
		background-color: #45a049;
		transform: translateY(-2px);
		box-shadow: 0 4px 15px rgba(76, 175, 80, 0.3);
	}

	.btn-secondary {
		background-color: transparent;
		color: white;
		padding: 1rem 2rem;
		text-decoration: none;
		border: 2px solid white;
		border-radius: 8px;
		font-weight: 600;
		font-size: 1.1rem;
		transition: all 0.3s;
		display: inline-block;
	}

	.btn-secondary:hover {
		background-color: white;
		color: #667eea;
		transform: translateY(-2px);
	}

	.btn-primary.large {
		padding: 1.25rem 2.5rem;
		font-size: 1.2rem;
	}

	.container {
		max-width: 1200px;
		margin: 0 auto;
		padding: 0 2rem;
	}

	section {
		margin-bottom: 5rem;
	}

	.intro {
		text-align: center;
		max-width: 800px;
		margin: 0 auto 5rem auto;
	}

	.intro h2 {
		font-size: 2.5rem;
		color: #333;
		margin-bottom: 1.5rem;
	}

	.intro p {
		font-size: 1.1rem;
		line-height: 1.8;
		color: #666;
	}

	.how-it-works h2 {
		text-align: center;
		font-size: 2.5rem;
		color: #333;
		margin-bottom: 3rem;
	}

	.steps {
		display: grid;
		grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
		gap: 2rem;
	}

	.step {
		text-align: center;
		padding: 2rem;
	}

	.step-number {
		width: 60px;
		height: 60px;
		background-color: #4caf50;
		color: white;
		border-radius: 50%;
		display: flex;
		align-items: center;
		justify-content: center;
		font-size: 1.5rem;
		font-weight: bold;
		margin: 0 auto 1.5rem auto;
	}

	.step h3 {
		font-size: 1.5rem;
		color: #333;
		margin-bottom: 1rem;
	}

	.step p {
		color: #666;
		line-height: 1.6;
	}

	.features h2 {
		text-align: center;
		font-size: 2.5rem;
		color: #333;
		margin-bottom: 3rem;
	}

	.feature-grid {
		display: grid;
		grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
		gap: 2rem;
	}

	.feature {
		background-color: #f8f9fa;
		padding: 2rem;
		border-radius: 12px;
		text-align: center;
		transition: transform 0.3s;
	}

	.feature:hover {
		transform: translateY(-5px);
		box-shadow: 0 10px 30px rgba(0, 0, 0, 0.1);
	}

	.feature-icon {
		font-size: 3rem;
		margin-bottom: 1rem;
	}

	.feature h3 {
		font-size: 1.3rem;
		color: #333;
		margin-bottom: 1rem;
	}

	.feature p {
		color: #666;
		line-height: 1.6;
	}

	.cta-section {
		background-color: #f8f9fa;
		padding: 4rem 2rem;
		border-radius: 16px;
		text-align: center;
	}

	.cta-section h2 {
		font-size: 2.5rem;
		color: #333;
		margin-bottom: 1.5rem;
	}

	.cta-section p {
		font-size: 1.1rem;
		color: #666;
		line-height: 1.8;
		max-width: 600px;
		margin: 0 auto 2.5rem auto;
	}

	@media (max-width: 768px) {
		.hero {
			padding: 4rem 2rem;
		}

		.hero-content h1 {
			font-size: 2.5rem;
		}

		.subtitle {
			font-size: 1.1rem;
		}

		.cta {
			flex-direction: column;
			align-items: center;
		}

		.intro h2,
		.how-it-works h2,
		.features h2,
		.cta-section h2 {
			font-size: 2rem;
		}

		.steps {
			grid-template-columns: 1fr;
		}

		.feature-grid {
			grid-template-columns: 1fr;
		}
	}
</style>
