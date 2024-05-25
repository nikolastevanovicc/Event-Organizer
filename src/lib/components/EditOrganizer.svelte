<script>
  import { FloatingLabelInput, Helper, Modal } from 'flowbite-svelte';
  import { List, Li, Card, Button, Navbar, NavBrand, NavLi, NavUl, NavHamburger, Input, Label, Heading, Fileupload } from 'flowbite-svelte';
  import { onMount } from 'svelte';
  import { ArrowRightOutline, HomeSolid, SearchOutline, TrashBinSolid, EditSolid, CirclePlusSolid, CashSolid  } from 'flowbite-svelte-icons';
  import { ExclamationCircleOutline } from 'flowbite-svelte-icons';
  export let organizerId = undefined;

  let festivals = [];

  let value;

  let organizer = {
      adresa: '',
      email: '',
      festivali: '',
      godinaOsnivanja: '',
      kontaktTelefon: '',
      logo: '',
      naziv: ''
    };
  onMount(async () => {
  const actualId = organizerId;
  const response = await fetch('https://event-organizer-dae1b-default-rtdb.europe-west1.firebasedatabase.app/organizatoriFestivala.json');
  const organizatoriFestivala = await response.json();

  Object.entries(organizatoriFestivala).forEach(([key, value]) => {
    if (key == actualId) {
      organizer = value;
    }
  });
  console.log("orgaaaaaaaaa",organizer);

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
  console.log(festivals);
});

  let deleteModal = false;
  let submitModal = false;
  let selectedFestival = null;

  function confirmDelete(festival) {
    selectedFestival = festival;
    deleteModal = true;
  }

  function deleteFestival() {
    // Ovde dodajte logiku za brisanje festivala
    console.log(`Deleted festival: ${selectedFestival.naziv}`);
    deleteModal = false;
  }


  function submitForm() {
  if (validateOrganizer(organizer)) {
    // Ovde dodajte logiku za podnošenje forme
    console.log("Form submitted");
    submitModal = false;
  }
}
  function validateOrganizer(organizer) {
    for (let key in organizer) {
      if (!organizer[key]) {
        alert(`Molimo unesite ${key}.`);
        return false;
      }
    }
    return true;
  }

  async function submitOrganizer(event) {
    event.preventDefault(); // sprečava osvežavanje stranice

    if (!validateOrganizer(organizer)) {
      return;
    }

    alert('Svi podaci su pravilno uneseni!');
  }

  function submitUser() {
  // Otvorite modal za potvrdu umesto da odmah submitujete formu
  submitModal = true;
}

function confirmSubmit() {
  // Ovde ide logika za submitovanje forme
  alert('Sve je u redu');
  submitModal = false;
}
</script>

<Heading class="flex flex-col items-center justify-center ml-10 mt-24" tag="h3">Kreiranje/izmena organizatora festivala</Heading>
<form class="mr-10 ml-10" on:submit={submitOrganizer}>
  <div class="mt-10 ml-5 mr-5 grid gap-6 mb-6 md:grid-cols-2">
    <div>
      <Label for="first_name" class="mb-2">Naziv Organizatora</Label>
      <Input bind:value={organizer.naziv} type="text" id="first_name" placeholder="" required />
    </div>
    <div>
      <Label for="last_name" class="mb-2">Adresa</Label>
      <Input bind:value={organizer.adresa} type="text" id="last_name" placeholder="" required />
    </div>
    <div>
      <Label for="company" class="mb-2">Godina Osnivanja</Label>
      <Input bind:value={organizer.godinaOsnivanja} type="text" id="company" placeholder="" required />
    </div>
    <div>
      <Label for="email" class="mb-2">Email</Label>
      <Input bind:value={organizer.email} type="email" id="email" placeholder="" required />
    </div>
    <div>
      <Label for="phone" class="mb-2">Kontakt Telefon</Label>
      <Input bind:value={organizer.kontaktTelefon} type="tel" id="phone" placeholder="" required />
    </div>
    <div>
      <Label for="logo" class="mb-2">Logo</Label>
      <Input type="file" id="logo" accept="image/*" required />
    </div>
    <Button class="mt-5" on:click={() => submitModal = true}>Submit</Button>
    </div>
</form>

<Modal bind:open={submitModal} size="xs" autoclose>
  <div class="text-center">
    <ExclamationCircleOutline class="mx-auto mb-4 text-gray-400 w-12 h-12 dark:text-gray-200" />
    <h3 class="mb-5 text-lg font-normal text-gray-500 dark:text-gray-400">Are you sure you want to submit this form?</h3>
    <Button color="red" class="me-2" on:click={submitForm}>Yes, I'm sure</Button>
    <Button color="alternative" on:click={() => submitModal = false}>No, cancel</Button>
  </div>
</Modal>

{#if organizerId !== 'add'}
<div class="flex justify-center mt-20">
  <Button class="mt-5">
    <CirclePlusSolid class="mr-1 w-3.5 h-3.5 ms-2 text-white" />
    <a href={`/addEvent/${organizerId}`}>Add Festival</a>
  </Button>
</div>
<div class="flex justify-center items-center my-14">
  <div class="grid md:grid-cols-4 sm:grid-cols-2 gap-4 w-[80%] mt-14">
    {#each festivals as festival (festival.id)}
      <Card img={festival.slike[0]}>
        <h5 class="mb-2 text-2xl font-bold tracking-tight text-gray-900 dark:text-white">{festival.naziv}</h5>
        <div class="flex items-center">
          <p class="font-bold ml-1 h-28 overflow-y-auto">{festival.opis}</p>
        </div>
        <Button class="mt-5" on:click={() => confirmDelete(festival)}>
          <TrashBinSolid class="w-3.5 h-3.5 ms-2 text-white" />
          Delete Festival
        </Button>
        <div class="flex items-col mt-3">
          <CashSolid color="green" class="mr-1" />
          <p class="font-bold">{festival.cena} RSD</p>
        </div>
      </Card>
    {/each}
  </div>
</div>

<Modal bind:open={deleteModal} size="xs" autoclose>
  <div class="text-center">
    <ExclamationCircleOutline class="mx-auto mb-4 text-gray-400 w-12 h-12 dark:text-gray-200" />
    <h3 class="mb-5 text-lg font-normal text-gray-500 dark:text-gray-400">Are you sure you want to delete this festival?</h3>
    <Button color="red" class="me-2" on:click={deleteFestival}>Yes, I'm sure</Button>
    <Button color="alternative" on:click={() => deleteModal = false}>No, cancel</Button>
  </div>
</Modal>
{/if}