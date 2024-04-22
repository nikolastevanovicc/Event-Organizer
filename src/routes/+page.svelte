<script>
  import { FloatingLabelInput, Helper } from 'flowbite-svelte';
  import { List, Li, Card, Button, Navbar, NavBrand, NavLi, NavUl, NavHamburger, Input } from 'flowbite-svelte';
  import { onMount } from 'svelte';
  import data from '../lib/assets/data.json';
  import { ArrowRightOutline, HomeSolid, SearchOutline  } from 'flowbite-svelte-icons';


  let organizers = [];
  // console.log(data.festivali)  

  // onMount(async () => {
  //   Object.values(data.organizatoriFestivala).forEach((value) => {
  //     organizers.push(value);
  //   });
  //   console.log("sdffd", organizers)
  //   organizers = organizers;
  // });
  onMount(async () => {
    Object.entries(data.organizatoriFestivala).forEach(([key, value]) => {
        organizers.push({ id: key, ...value });
    });

    organizers = organizers;
});
</script>
<div class="flex justify-center items-center my-14">
  <div class="grid md:grid-cols-4 sm:grid-cols-2 gap-4 w-[80%] mt-14">
    {#each Object.entries(organizers) as [key, organizer] (key)}
    <Card img={organizer.logo}>
      <h5 class="mb-2 text-2xl font-bold tracking-tight text-gray-900 dark:text-white">{organizer.naziv}</h5>
      <div class="flex items-center">
          <HomeSolid color="green"></HomeSolid>
          <p class="font-bold ml-1">{organizer.adresa}</p>
      </div>
        <Button class = "mt-5">
          <a href="/organizer/{organizer.id}">Read more <ArrowRightOutline class="w-3.5 h-3.5 ms-2 text-white" /></a>
        </Button>
    </Card>
    {/each}
  </div>
</div>

<style>
   
</style>
