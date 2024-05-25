<script>
    import { Img, TextPlaceholder } from 'flowbite-svelte';
    import { List, Li, Card, Button, Navbar, NavBrand, NavLi, NavUl, NavHamburger, Input, Heading, P, Carousel  } from 'flowbite-svelte';
    import { ArrowLeftOutline, ArrowRightOutline, CashSolid, SearchOutline  } from 'flowbite-svelte-icons';
    import { onMount } from 'svelte';
    export let data;

    let trenutnaSlika = 0;
  let festival = null;

  onMount(async () => {
    const actualId = data.id;
    const response = await fetch('https://event-organizer-dae1b-default-rtdb.europe-west1.firebasedatabase.app/festivali.json');
    const festivali = await response.json();
    
    Object.values(festivali).forEach(value => {
      Object.entries(value).forEach(([key, value2]) => {
        if (key == actualId) {
          festival = value2;
        }
      });
    });
    console.log("festival", festival);
  });



</script>

<Navbar class = "fixed w-full top-0 left-0 bg-[#e5e7eb] bg-opacity-95">
  <NavBrand href="/">
    <img src="../src/lib/assets/logo-no-background.png" class="me-3 h-7 sm:h-14" alt="Event Organizers" />
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
      <Input id="search-navbar" class="ps-10" placeholder="Search..." />
    </div>
    <NavHamburger />
  </div>
  <NavUl>
    <NavLi href="/" active={true}>Home</NavLi>
    <NavLi href="/about">About</NavLi>
    <NavLi href="/adminOrganizer/1">Admin Organizer</NavLi>
    <NavLi href="/adminUser/1">Admin User</NavLi>
  </NavUl>
</Navbar>
  <!-- /src/lib/assets/logo-no-background.png -->
  <div class="flex flex-col items-center mt-20 bg-opacity-95">
    <div class="w-full h-[400px] relative">
        <img src={festival ? festival.slike[1] : 'Loading...'} class="h-full w-full object-cover" alt="sample 1" />
    </div>
    <h1 class="text-4xl text-left py-4 mb-10">{festival ? festival.naziv : 'Loading...'}</h1>
    <div class="w-full md:w-3/4 lg:w-1/2 mx-auto ">
        <P class="bg-neutral-200 mb-3 border p-4 rounded-md" weight="light" color="text-gray-500 dark:text-gray-400">
          {festival ? festival.opis : 'Loading...'}
        </P>
        <div class = "flex flex-wrap justify-between mt-3">
          <P class="bg-neutral-200 mb-3 border p-4 rounded-md flex items-col" weight="light" color="text-gray-500 dark:text-gray-400">
            <p class = "font-bold mr-1">Cena:  </p>
            <p class = "font-bold">{festival ? festival.cena : 'Loading...'} RSD</p>
          </P>
          <P class="bg-neutral-200 mb-3 border p-4 rounded-md flex items-col" weight="light" color="text-gray-500 dark:text-gray-400">
            <p class = "font-bold mr-1">Prevoz:  </p>
            <p class = "font-bold">{festival ? festival.prevoz : 'Loading...'}</p>
          </P>
          <P class="bg-neutral-200 mb-3 border p-4 rounded-md flex items-col" weight="light" color="text-gray-500 dark:text-gray-400">
            <p class = "font-bold mr-1">Tip festivala:  </p>
            <p class = "font-bold">{festival ? festival.tip : 'Loading...'}</p>
          </P>
          <P class="bg-neutral-200 mb-3 border p-4 rounded-md flex items-col" weight="light" color="text-gray-500 dark:text-gray-400">
            <p class = "font-bold mr-1">Maksimalan broj osoba:  </p>
            <p class = "font-bold">{festival ? festival.maxOsoba : 'Loading...'}</p>
          </P>

        </div>
     </div>
     <div class="relative w-full md:w-3/4 lg:w-1/2 mx-auto h-300 mb-14 flex items-center ">
      <!-- Ostatak koda... -->
      <div class="relative mt-3">
          <button class="absolute left-5 top-1/2 transform -translate-y-1/2 bg-white bg-opacity-50 p-3 rounded-full ml-2" on:click={() => trenutnaSlika = Math.max(0, trenutnaSlika - 1)}>
              <ArrowLeftOutline />
          </button>
          {#if festival && festival.slike && festival.slike.length > 0}
              <img src={festival.slike[trenutnaSlika]} alt="Festival slika" class="mr-3 w-full object-cover shadow-lg rounded-md">
          {:else}
              <p>Loading...</p>
          {/if}
          <button class="absolute right-0 top-1/2 transform -translate-y-1/2 bg-white bg-opacity-50 p-3 rounded-full mr-2" on:click={() => trenutnaSlika = Math.min((festival.slike.length - 1), trenutnaSlika + 1)}>
              <ArrowRightOutline />
          </button>
      </div>
  </div>
</div>
