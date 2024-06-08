<script>
  import { FloatingLabelInput, Helper } from 'flowbite-svelte';
  import { List, Li, Card, Button, Navbar, NavBrand, NavLi, NavUl, NavHamburger, Input, P } from 'flowbite-svelte';
  import { onMount } from 'svelte';
  import { ArrowRightOutline, HomeSolid, SearchOutline, CashSolid, MailBoxSolid, PhoneSolid } from 'flowbite-svelte-icons';
  import Highlight from '/src/lib/components/Highlight.svelte';

  export let data;
  let festivals = [];
  let formModal = false;
  let searchQuery = '';

  function closeAndNavigate() {
    formModal = false;
    window.location.href = '../editUser/add';
  }

  let organizer;
  onMount(async () => {
    const actualId = data.id;
    const response = await fetch('https://event-organizer-dae1b-default-rtdb.europe-west1.firebasedatabase.app/organizatoriFestivala.json');
    const organizatoriFestivala = await response.json();

    Object.entries(organizatoriFestivala).forEach(([key, value]) => {
      if (key == actualId) {
        organizer = value;
      }
    });

    if (organizer && organizer.festivali) {
      const responseFestivals = await fetch('https://event-organizer-dae1b-default-rtdb.europe-west1.firebasedatabase.app/festivali.json');
      const festivali = await responseFestivals.json();

      Object.entries(festivali).forEach(([key, value]) => {
        if (key == organizer.festivali) {
          Object.entries(value).forEach(([key2, value2]) => {
            const festival = { id: key2, ...value2 };
            festivals = [...festivals, festival]; // Update reactively
          });
        }
      });
    }
  });

  $: filteredFestivals = festivals.filter(festival => {
    const lowerCaseQuery = searchQuery.toLowerCase();
    return festival.naziv.toLowerCase().includes(lowerCaseQuery) || festival.tip.toLowerCase().includes(lowerCaseQuery) || festival.opis.toLowerCase().includes(lowerCaseQuery);
  });
</script>

<Navbar class="fixed w-full top-0 left-0 bg-[#e5e7eb] bg-opacity-95">
  <NavBrand href="/">
    <img src="\src\lib\assets\logo-no-background.png" class="me-3 h-7 sm:h-14" alt="Event Organizers" />
    <span class="self-center whitespace-nowrap text-xl font-semibold dark:text-white">Event Organizers</span>
  </NavBrand>
  <div class="flex md:order-2">
    <Button color="none" data-collapse-toggle="mobile-menu-3" aria-controls="mobile-menu-3" aria-expanded="false" class="md:hidden text-gray-500 dark:text-gray-400 hover:bg-gray-100 dark:hover:bg-gray-700 focus:outline-none focus:ring-4 focus:ring-gray-200 dark:focus:ring-gray-700 rounded-lg text-sm p-2.5 me-1">
      <SearchOutline class="w-5 h-5" />
    </Button>
    <div class="hidden relative md:block">
      <div class="flex absolute inset-y-0 start-0 items-center ps-3 pointer-events-none">
        <SearchOutline class="w-4 h-4" />
      </div>
      <Input id="search-navbar" class="ps-10" placeholder="Search..." bind:value={searchQuery} />
    </div>
    <NavHamburger />
  </div>
  <NavUl>
    <NavLi href="/" active={true}>Home</NavLi>
    <NavLi href="/adminOrganizer/1">Admin Organizer</NavLi>
    <NavLi href="/adminUser/1">Admin User</NavLi>
    <NavLi on:click={() => (formModal = true)}>Login</NavLi>
  </NavUl>
</Navbar>

<div class="flex flex-col items-center mt-20 bg-opacity-95">
  <div class="w-full h-[400px] relative">
    <img src={organizer ? organizer.logo : 'Loading...'} class="h-full w-full object-cover" alt="sample 1" />
  </div>
  <h1 class="text-4xl text-left py-4 mb-10">{organizer ? organizer.naziv : 'Loading...'}</h1>
  <div class="w-full md:w-3/4 lg:w-1/2 mx-auto">
    <P class="bg-neutral-200 mb-3 border p-4 rounded-md flex flex-items-col flex space-x-4" weight="light" color="text-gray-500 dark:text-gray-400">
      <HomeSolid color="green" class="mr-2"></HomeSolid>
      {organizer ? organizer.adresa : 'Loading...'}
    </P>
    <div class="flex flex-wrap justify-between mt-3">
      <P class="bg-neutral-200 mb-3 border p-4 rounded-md flex items-col" weight="light" color="text-gray-500 dark:text-gray-400">
        <p class="font-bold mr-2">Godina osnivanja:</p>
        <p class="font-bold">{organizer ? organizer.godinaOsnivanja : 'Loading...'}</p>
      </P>
      <P class="bg-neutral-200 mb-3 border p-4 rounded-md flex items-col" weight="light" color="text-gray-500 dark:text-gray-400">
        <PhoneSolid color="green" class="mr-2"></PhoneSolid>
        <p class="font-bold">{organizer ? organizer.kontaktTelefon : 'Loading...'}</p>
      </P>
      <P class="bg-neutral-200 mb-3 border p-4 rounded-md flex items-col" weight="light" color="text-gray-500 dark:text-gray-400">
        <MailBoxSolid color="green" class="mr-2"></MailBoxSolid>
        <p class="font-bold">{organizer ? organizer.email : 'Loading...'}</p>
      </P>
    </div>
  </div>
</div>

<div class="flex justify-center items-center my-14">
  <div class="grid md:grid-cols-4 sm:grid-cols-2 gap-4 w-[80%] mt-14">
    {#each filteredFestivals as festival}
      <Card img={festival.slike[0]}>
        <h5 class="mb-2 text-2xl font-bold tracking-tight text-gray-900 dark:text-white">
          <Highlight text={festival.naziv} query={searchQuery}/>
        </h5>
        <div class="flex items-center">
          <p class="font-bold ml-1 h-28 overflow-y-auto">
            <Highlight text={festival.opis} query={searchQuery}/>
          </p>
        </div>
        <Button class="mt-5">
          <a href="/event/{festival.id}">Read more <ArrowRightOutline class="w-3.5 h-3.5 ms-2 text-white" /></a>
        </Button>
        <div class="flex items-col mt-3">
          <CashSolid color="green" class="mr-1"/>
          <p class="font-bold">{festival.cena} RSD</p>
        </div>
      </Card>
    {/each}
  </div>
</div>
