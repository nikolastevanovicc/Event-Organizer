<script>
  import { FloatingLabelInput, Helper } from 'flowbite-svelte';
  import { List, Li, Card, Button, Navbar, NavBrand, NavLi, NavUl, NavHamburger, Input,P } from 'flowbite-svelte';
  import { onMount } from 'svelte';
  import { ArrowRightOutline, HomeSolid, SearchOutline, CashSolid, MailBoxSolid,PhoneSolid  } from 'flowbite-svelte-icons';
  export let data;
  let festivals = [];


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
</script>


<div class="flex flex-col items-center mt-20 bg-opacity-95">
    <div class="w-full h-[400px] relative">
        <img src={organizer ? organizer.logo : 'Loading...'} class="h-full w-full object-cover" alt="sample 1" />
    </div>
    <h1 class="text-4xl text-left py-4 mb-10">{organizer ? organizer.naziv : 'Loading...'}</h1>
    <div class="w-full md:w-3/4 lg:w-1/2 mx-auto ">
            <P class="bg-neutral-200 mb-3 border p-4 rounded-md flex flex-items-col flex space-x-4" weight="light" color="text-gray-500 dark:text-gray-400">
                <HomeSolid color="green" class = "mr-2"></HomeSolid>
                {organizer ? organizer.adresa : 'Loading...'}
            </P>
        <div class = "flex flex-wrap justify-between mt-3">
          <P class="bg-neutral-200 mb-3 border p-4 rounded-md flex items-col" weight="light" color="text-gray-500 dark:text-gray-400">
            <p class = "font-bold mr-2">Godina osnivanja:  </p>
            <p class = "font-bold">{organizer ? organizer.godinaOsnivanja : 'Loading...'}.</p>
          </P>
          <P class="bg-neutral-200 mb-3 border p-4 rounded-md flex items-col" weight="light" color="text-gray-500 dark:text-gray-400">
            <PhoneSolid color="green" class = "mr-2"></PhoneSolid>
            <p class = "font-bold">{organizer ? organizer.kontaktTelefon : 'Loading...'}</p>
          </P>
          <P class="bg-neutral-200 mb-3 border p-4 rounded-md flex items-col" weight="light" color="text-gray-500 dark:text-gray-400">
            <MailBoxSolid color="green" class = "mr-2"></MailBoxSolid>
            <p class = "font-bold">{organizer ? organizer.email : 'Loading...'}</p>
          </P>

        </div>
     </div>
</div>
  <div class=" flex justify-center items-center my-14">
    <div class="grid md:grid-cols-4 sm:grid-cols-2 gap-4 w-[80%] mt-14">
      {#each festivals as festival}
      <Card img={festival.slike[0]}>
        <h5 class="mb-2 text-2xl font-bold tracking-tight text-gray-900 dark:text-white">{festival.naziv}</h5>
        <div class="flex items-center">
            <p class="font-bold ml-1 h-28 overflow-y-auto">{festival.opis}</p>
        </div>
          <Button class = "mt-5">
            <a href="/event/{festival.id}">Read more <ArrowRightOutline class="w-3.5 h-3.5 ms-2 text-white" /></a>
          </Button>
        <div class = "flex items-col mt-3">
            <CashSolid color="green" class = "mr-1"/>
            <p class = "font-bold">{festival.cena} RSD</p>
        </div>
      </Card>
    {/each}
    </div>
  </div>
  <style>
     
  </style>