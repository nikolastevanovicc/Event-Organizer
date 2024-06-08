<script>
  import { FloatingLabelInput, Helper, Button, Navbar, NavBrand, NavLi, NavUl, NavHamburger, Input, Modal, Label, Card } from 'flowbite-svelte';
  import { onMount } from 'svelte';
  import { ArrowRightOutline, HomeSolid, SearchOutline } from 'flowbite-svelte-icons';
  import { writable } from 'svelte/store';

  let formModal = false;
  let organizers = writable([]);
  let filteredOrganizers = writable([]);
  let searchQuery = '';

  function closeAndNavigate() {
    formModal = false;
    window.location.href = '../editUser/add';
  }

  onMount(async () => {
    const response = await fetch('https://event-organizer-dae1b-default-rtdb.europe-west1.firebasedatabase.app/organizatoriFestivala.json');
    const data = await response.json();
    const orgList = Object.entries(data).map(([key, value]) => ({ id: key, ...value }));
    organizers.set(orgList);
    filteredOrganizers.set(orgList);
  });

  $: if (searchQuery) {
    filteredOrganizers.set($organizers.filter(org => org.naziv.toLowerCase().includes(searchQuery.toLowerCase())));
  } else {
    filteredOrganizers.set($organizers);
  }
</script>

<Navbar class="fixed w-full top-0 left-0 bg-[#e5e7eb] bg-opacity-95">
  <NavBrand href="/">
    <img src="/src/lib/assets/logo-no-background.png" class="me-3 h-7 sm:h-14" alt="Event Organizers" />
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

<Modal bind:open={formModal} size="xs" autoclose={false} class="w-full">
  <form class="flex flex-col space-y-6" action="#">
    <h3 class="mb-4 text-xl font-medium text-gray-900 dark:text-white">Sign in to our platform</h3>
    <Label class="space-y-2">
      <span>Email</span>
      <Input type="email" name="email" placeholder="name@company.com" required />
    </Label>
    <Label class="space-y-2">
      <span>Your password</span>
      <Input type="password" name="password" placeholder="•••••" required />
    </Label>
    <Button type="submit" class="w-full1">Login to your account</Button>
    <div class="text-sm font-medium text-gray-500 dark:text-gray-300">
      Not registered? <button on:click={closeAndNavigate} class="text-primary-700 hover:underline dark:text-primary-500"> Create account </button>
    </div>
  </form>
</Modal>

<div class="flex justify-center items-center my-14">
  <div class="w-[80%] mt-14">
    <div class="grid md:grid-cols-4 sm:grid-cols-2 gap-4">
      {#each $filteredOrganizers as organizer (organizer.id)}
        <Card img={organizer.logo}>
          <h5 class="mb-2 text-2xl font-bold tracking-tight text-gray-900 dark:text-white">{organizer.naziv}</h5>
          <div class="flex items-center">
            <HomeSolid color="green" />
            <p class="font-bold ml-1">{organizer.adresa}</p>
          </div>
          <Button class="mt-5">
            <a href="/organizer/{organizer.id}">Read more <ArrowRightOutline class="w-3.5 h-3.5 ms-2 text-white" /></a>
          </Button>
        </Card>
      {/each}
    </div>
  </div>
</div>

<style>
</style>
