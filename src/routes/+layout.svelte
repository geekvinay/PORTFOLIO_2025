<script lang="ts">
	import '../app.css';
	import { Separator } from '$lib/components/ui/separator';
	import { mode, ModeWatcher, toggleMode } from 'mode-watcher';
	import Icon from '@iconify/svelte';
	import TypedText from '../components/TypedText.svelte';
	import { onMount } from 'svelte';

	export let children;

	const linkedin = import.meta.env.VITE_SOCIALS_LINKEDIN;
	const github = import.meta.env.VITE_SOCIALS_GITHUB;
	const instagram = import.meta.env.VITE_SOCIALS_INSTAGRAM;
	const pageTitle = 'Vinay Kishore - Software Developer @ Quizizz';

	// Bangalore coordinates
	const latitude = 12.9716;
	const longitude = 77.5946;

	let location = 'Fetching location...';
	let temperature = '--';
	let weatherDescription = 'Fetching weather...';
	let localTime = '--';
	let weatherSummary = 'Fetching summary...';

	const getWeatherAndTime = async () => {
		try {
			const apiKey = 'b3b631d8e277ca3f745a22902d340564';
			const weatherUrl = `https://api.openweathermap.org/data/2.5/weather?lat=${latitude}&lon=${longitude}&units=metric&appid=${apiKey}`;

			const weatherResponse = await fetch(weatherUrl);
			const weatherData = await weatherResponse.json();

			if (weatherData.cod !== 200) {
				throw new Error(weatherData.message);
			}

			location = weatherData.name; // City name
			temperature = `${weatherData.main.temp.toString().slice(0, 2)}°C`;
			weatherDescription = weatherData.weather[0].description;

			// Bangalore's timezone offset in seconds (UTC + 5:30)
			const timezoneOffsetInSeconds = 19800; // 5 hours 30 minutes

			// Get UTC time and adjust for Bangalore timezone
			const utcTime = new Date(); // Current local time
			const utcTimeInMilliseconds = utcTime.getTime() + utcTime.getTimezoneOffset() * 60000; // Convert local time to UTC (milliseconds)

			// Now apply the Bangalore timezone offset
			const localTimeInMilliseconds = utcTimeInMilliseconds + timezoneOffsetInSeconds * 1000;

			// Convert to local Bangalore time
			const localTimeDate = new Date(localTimeInMilliseconds);
			localTime = localTimeDate.toLocaleTimeString('en-US', {
				hour: '2-digit',
				minute: '2-digit',
				hour12: true
			});
		} catch (error) {
			console.error('Error fetching weather or time:', error);
			location = 'Error fetching location';
			weatherDescription = 'Error fetching weather';
			temperature = '--';
			localTime = '--';
		}
	};

	onMount(() => {
		getWeatherAndTime();
	});
</script>

<ModeWatcher defaultMode="dark" />
<svelte:head>
	<title>{pageTitle}</title>
</svelte:head>
<section
	class="root flex min-h-screen flex-col items-center justify-start gap-4 border-x md:mx-[5vw]"
>
	<header class="flex min-h-[3rem] w-full flex-col justify-between">
		<section class="flex items-center justify-between px-8 py-6">
			<a href="/" class="hero-link boxy-font text-2xl font-bold uppercase">Kishore Vinay</a>

			<div class="links flex list-none items-center justify-between gap-6 font-medium">
				<button class="hidden opacity-70 md:block">projects</button>
				<button class="hidden opacity-70 md:block">blogs</button>
				<button class="hidden opacity-70 md:block">about</button>

				<div class="icon-wrapper ml-8 flex items-center justify-between gap-4 opacity-85">
					<a
						href={linkedin || 'https://www.linkedin.com/in/vinaykishore/'}
						target="_blank"
						class="transition-all duration-300 hover:scale-110 hover:opacity-95"
					>
						<Icon icon={'ant-design:linkedin-filled'} class="text-2xl" />
					</a>
					<a
						href={github || 'https://github.com/geekvinay'}
						target="_blank"
						class="transition-all duration-300 hover:scale-110 hover:opacity-95"
					>
						<Icon icon={'ant-design:github-outlined'} class="text-2xl" />
					</a>
					<a
						href={instagram || 'https://instagram.com/_vinaykishore'}
						target="_blank"
						class="transition-all duration-300 hover:scale-110 hover:opacity-95"
					>
						<Icon icon={'ant-design:instagram-outlined'} class="text-2xl" />
					</a>
					<button onclick={toggleMode} class="transition-all duration-300 hover:scale-110 hover:opacity-95">
						<Icon
							icon={$mode == 'light' ? 'ant-design:sun-filled' : 'ant-design:moon-filled'}
							class="text-2xl"
						/>
					</button>
				</div>
			</div>
		</section>

		<Separator />

		<div
			class="interactive-elements mono-font flex w-full justify-between px-8 py-4 text-sm font-thin"
		>
			<p class="message hidden text-sm opacity-70 sm:block">
				<TypedText
					strings={[
						'Oh, Hello There!! 👻',
						'How are you doing?? 😄',
						'Hope everything is going great!! 🤞'
					]}
				/>
			</p>

			<div class="geo-data flex w-full justify-between gap-6 font-thin opacity-70 sm:w-fit">
				<p class="flex items-center gap-2">
					<Icon icon="ant-design:signal-filled" class="animate-pulse text-green-500" />
					{location}
				</p>
				<p class="hidden sm:block animate-pulse">{localTime}</p>
				<p class="flex items-center gap-1">
					<Icon icon="ant-design:cloud-filled" class="text-xl text-blue-300" />
					{temperature}
				</p>
			</div>
		</div>
	</header>
	{@render children()}
</section>
