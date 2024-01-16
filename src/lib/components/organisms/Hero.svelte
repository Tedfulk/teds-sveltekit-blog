<script>

	import GitHubIcon from '$lib/icons/socials/github.svelte';
	import Button from '$lib/components/atoms/Button.svelte';
	import Sparkles from '../atoms/Sparkles.svelte';
	import SparklingHighlight from '../molecules/SparklingHighlight.svelte';
</script>

<section id="hero">
		<h1 class="hello">Ted's Tech <SparklingHighlight color="primary">Chronicles!</SparklingHighlight></h1>
		<p class="intro">
			<span class="left">"Clean code always looks like it was written by someone who cares." </span>
			<span class="right">— Robert C. Martin</span>
		</p>
	<div class="ctas">
		<Sparkles >
			<Button href="https://github.com/Tedfulk/" color="secondary" style="understated" >
				<GitHubIcon slot="icon" />
				GitHub
			</Button>
		</Sparkles>
	</div>
</section>

<style lang="scss">
	@import '$lib/scss/breakpoints.scss';

	#hero {
		display: flex;
		flex-direction: column;
		justify-content: center;
		align-items: center;
		gap: 15px;
		position: relative;
		padding-top: 50px;
		padding-bottom: 100px;

		@include for-phone-only {
			padding: 40px 0 50px;
		}

		.hello {
			text-align: center;
		}

		.intro {
			font-weight: 500;
			font-size: 1.4rem;
			width: min(100%, 440px);
			display: flex;
			flex-direction: column;

			.left {
				text-align: left;
				font-style: italic;
			}
			.right {
				text-align: right;
			}

			@include for-phone-only {
				display: none;
			}
		}

		.ctas {
			display: flex;
			flex-wrap: wrap;
			align-items: flex-end;
			justify-content: center;
			gap: 10px;
			padding-top: 3.5rem;
			width: 100%;
		}
	}
</style>
