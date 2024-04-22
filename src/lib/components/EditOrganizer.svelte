<script>
    import { FloatingLabelInput, Helper } from 'flowbite-svelte';
    import { List, Li, Card, Button, Navbar, NavBrand, NavLi, NavUl, NavHamburger, Input, Label, Heading, Fileupload } from 'flowbite-svelte';
    import { onMount } from 'svelte';
    import dataa from '$lib/assets/data.json';
    import { ArrowRightOutline, HomeSolid, SearchOutline, TrashBinSolid, EditSolid, CirclePlusSolid, CashSolid  } from 'flowbite-svelte-icons';

    export let organizerId = undefined;

    let festivals = [];

    let organizer;
    let value;

    onMount(async () => {
  const actualId = organizerId
  Object.entries(dataa.organizatoriFestivala).forEach(([key, value]) => {
    if (key == actualId) {
      organizer = value;
    }
    organizer = organizer; 
    console.log("organizer", organizer)
  });

  if (organizer && organizer.festivali) {
    Object.entries(dataa.festivali).forEach(([key, value]) => {
      if (key == organizer.festivali) {
        Object.entries(value).forEach(([key2, value2]) => {
          const festival = { id: key2, ...value2 }; // Dodajemo ključ kao id i širimo sve ostale vrednosti
          festivals.push(festival);
          festivals = festivals;
        });
      }
    });
  }

  console.log("festivalsssssssss", festivals)
});
</script>

  <Heading class = "flex flex-col items-center justify-center ml-10 mt-24 "tag="h3">Kreiranje/izmena organizatora festivala</Heading>
<form class = "mr-10 ml-10">
    <div class="mt-10 ml-5 mr-5 grid gap-6 mb-6 md:grid-cols-2">
      <div>
        <Label for="first_name" class="mb-2">Naziv Organizatora</Label>
        <Input value={organizerId === 'add' || !organizer ? '' : organizer.naziv} type="text" id="first_name" placeholder="" required/>
      </div>
      <div >
        <Label for="last_name" class="mb-2">Adresa</Label>
        <Input type="text" id="last_name" placeholder="" required  value={organizerId === 'add' || !organizer ? '' : organizer.adresa}/>
      </div>
      <div>
        <Label for="company" class="mb-2">Godina Osnivanja</Label>
        <Input type="text" id="company" placeholder="" required  value={organizerId === 'add' || !organizer ? '' : organizer.godinaOsnivanja} />
      </div>
      <div>
        <Label for="phone" class="mb-2">Broj Telefona</Label>
        <Input type="tel" id="phone" placeholder="123-45-678" pattern={"[0-9]{3}-[0-9]{2}-[0-9]{3}"} required  value={organizerId === 'add' || !organizer ? '' : organizer.kontaktTelefon} />
      </div>
      <div>
        <Label for="website" class="mb-2">Email adresa</Label>
        <Input type="url" id="website" placeholder="kontakt@gmail.com" required  value={organizerId === 'add' || !organizer ? '' : organizer.email}/>
      </div>
    </div>
    <Label class="space-y-2 mb-2 ml-5">
        <span>Upload file</span>
        <Fileupload  bind:value />
      </Label>
    <div class="mb-6 ml-5 mr-5">
    <Button class= "ml-5 mr-5" type="submit">Submit</Button>
</form>
{#if organizerId !==  "add"}
<div class="flex justify-center mt-20">
    <Button class = "mt-5">
        <CirclePlusSolid class=" mr-1 w-3.5 h-3.5 ms-2 text-white" />
        <a href="/addEvent/1">Add Festival</a>
      </Button>
</div>  
  <div class="flex justify-center items-center my-14">
    <div class="grid md:grid-cols-4 sm:grid-cols-2 gap-4 w-[80%] mt-14">
      {#each Object.entries(festivals) as [key, festival] (key)}
      <Card img={festival.slike[0]}>
        <h5 class="mb-2 text-2xl font-bold tracking-tight text-gray-900 dark:text-white">{festival.naziv}</h5>
        <div class="flex items-center">
            <p class="font-bold ml-1 h-28 overflow-y-auto">{festival.opis}</p>
        </div>
          <Button class = "mt-5">
            <TrashBinSolid class="w-3.5 h-3.5 ms-2 text-white" />
            <a href="/organizer/1">Delete Festival</a>
          </Button>
        <div class = "flex items-col mt-3">
            <CashSolid color="green" class = "mr-1"/>
            <p class = "font-bold">{festival.cena} RSD</p>
        </div>
      </Card>
      {/each}
    </div>
</div>
{/if}