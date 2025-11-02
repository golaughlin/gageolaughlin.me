<script>
    import '../styles/global.css';
    import { onMount } from 'svelte';

    // Set the date of the lockout.
    const lockoutDate = new Date(2026, 11, 1, 0, 0).getTime();
    
    // Set the current date.
    let currentDate = $state(new Date().getTime());

    // Find the distence between now and the lockout.
    const distance = $derived(lockoutDate - currentDate);

    // Time calculations for days, hours, minutes, and seconds.
    const days = $derived(Math.floor(distance / (1000 * 60 * 60 * 24)));
    const hours = $derived(Math.floor((distance % (1000 * 60 * 60 * 24)) / (1000 * 60 * 60)));
    const minutes = $derived(Math.floor((distance % (1000 * 60 * 60)) / (1000 * 60)));
    const seconds = $derived(Math.floor((distance % (1000 * 60)) / 1000));

    // Update the countdown every 1 second.
    onMount(() => {
        const interval = setInterval(() => {
            currentDate = new Date().getTime();
        }, 1000);
        return () => clearInterval(interval);
    });
    
</script>

<div class="flex items-center justify-center gap-8" aria-live="polite">
    {#if distance > 0}
        <div class="flex flex-col items-center justify-center gap-2">
            <div class="font-medium text-sm text-center uppercase">Days</div>
            <div class="font-extrabold text-center text-7xl tabular-nums">{days}</div>
        </div>
        <div class="flex flex-col items-center justify-center gap-2">
            <div class="font-medium text-sm text-center uppercase">Hours</div>
            <div class="font-extrabold text-center text-7xl tabular-nums">{hours}</div>
        </div>
        <div class="flex flex-col items-center justify-center gap-2">
            <div class="font-medium text-sm text-center uppercase">Minutes</div>
            <div class="font-extrabold text-center text-7xl tabular-nums">{minutes}</div>
        </div>
        <div class="flex flex-col items-center justify-center gap-2">
            <div class="font-medium text-sm text-center uppercase">Seconds</div>
            <div class="font-extrabold text-center text-7xl tabular-nums">{seconds}</div>
        </div>
    {:else}
        <div class="flex flex-col items-center justify-center">
            <iframe width="560" height="315" src="https://www.youtube.com/embed/Kx8tYMEXEeQ?si=vXGPsvMvYOUMVZnW" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>
        </div>
    {/if}
</div>