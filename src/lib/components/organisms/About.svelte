<script>
	import SparklingHighlight from '$lib/components/molecules/SparklingHighlight.svelte';
	import Socials from '$lib/components/molecules/Socials.svelte';
	import Image from '../atoms/Image.svelte';
</script>

<section id="about">
	<div class="info">
		<!-- <h2>
			About: 
			<span class="highlight">Ted's Tech</span>
			<SparklingHighlight color="primary">Chronicles!</SparklingHighlight>
		</h2> -->
		
		<p>
			Welcome to a curated space where technology meets practicality. Here, I explore into the latest advancements and offer insights into the tools and tech that power our digital world. 
			<br />
			<br />
			From in-depth articles on Python's newest libraries to exploring the backbone of robust backend systems, and the ever-evolving landscape of web development - this is your go-to source for tech that matters. Join me on this journey as we dissect news articles, analyze informative YouTube videos, and explore the potentials of AI. 
		</p>
		<div class="socials">
			<span>Socials:</span>
			<Socials />
		</div>
	</div>
	<div class="image">
		<Image
			src="/images/fresh-idea-1.png"
			alt="Sample for the static template"
		/>
	</div>
</section>

<style lang="scss">
	@import '$lib/scss/breakpoints.scss';

	#about {
		position: relative;
		display: grid;
		grid-template-columns: 500px 250px;
		align-items: center;
		justify-content: space-evenly;
		padding-bottom: 50px;

		@include for-phone-only {
			grid-template-columns: 1fr;
			justify-items: center;
			gap: 20px;
		}

		.info {
			display: flex;
			flex-direction: column;
			gap: 10px;
			h2 {
				@include for-phone-only {
					text-align: center;
				}
			}

			p {
				@include for-phone-only {
					text-align: justify;
				}
			}

			@include for-phone-only {
				gap: 20px;
			}
		}

		.socials {
			display: flex;
			align-items: center;
			gap: 15px;

			@include for-phone-only {
				justify-content: center;
				margin-bottom: 10px;
				span {
					display: none;
				}
			}
		}

		.image {
			width: 320px;
			height: 320px;
		}
	}
</style>
