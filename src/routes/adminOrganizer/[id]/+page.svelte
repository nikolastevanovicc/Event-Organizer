<script>
    import { FloatingLabelInput, Helper } from 'flowbite-svelte';
    import { List, Li, Card, Button, Navbar, NavBrand, NavLi, NavUl, NavHamburger, Input } from 'flowbite-svelte';
    import { onMount } from 'svelte';
    import { ArrowRightOutline, HomeSolid, SearchOutline, TrashBinSolid, EditSolid, CirclePlusSolid  } from 'flowbite-svelte-icons';
    import { Modal } from 'flowbite-svelte';
    import { ExclamationCircleOutline } from 'flowbite-svelte-icons';

    let popupModal = false;
    let selectedOrganizer = null;
  
    let organizers = [];

onMount(async () => {
  const response = await fetch('https://event-organizer-dae1b-default-rtdb.europe-west1.firebasedatabase.app/organizatoriFestivala.json');
  const organizatoriFestivala = await response.json();
  
  organizers = Object.entries(organizatoriFestivala).map(([id, value]) => ({ id, ...value }));
  console.log("organizers", organizers);
});
  function confirmDelete(organizer) {
    selectedOrganizer = organizer;
    popupModal = true;
  }
  function deleteOrganizer() {
    // Ovde dodajte logiku za brisanje organizatora
    console.log(`Deleted organizer: ${selectedOrganizer.id}`);
    popupModal = false;
  }
  </script>
  <div class="flex justify-center mt-20">
    <Button class = "mt-5">
        <CirclePlusSolid class=" mr-1 w-3.5 h-3.5 ms-2 text-white" />
        <a href="/editOrganizer/add">Add Organizer</a>
      </Button>
</div>
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
            <EditSolid class="w-3.5 h-3.5 ms-2 text-white" />
            <a href="/editOrganizer/{organizer.id}">Edit</a>
          </Button>
          <Button class="mt-5" on:click={() => confirmDelete(organizer)}>
            <TrashBinSolid class="w-3.5 h-3.5 ms-2 text-white" />
            Delete Organizer
          </Button>
      </Card>
      {/each}
    </div>
  </div>
  <Modal bind:open={popupModal} size="xs" autoclose>
    <div class="text-center">
      <ExclamationCircleOutline class="mx-auto mb-4 text-gray-400 w-12 h-12 dark:text-gray-200" />
      <h3 class="mb-5 text-lg font-normal text-gray-500 dark:text-gray-400">Are you sure you want to delete this product?</h3>
      <Button color="red" class="me-2" on:click={deleteOrganizer}>Yes, I'm sure</Button>
      <Button color="alternative" on:click={() => popupModal = false}>No, cancel</Button>
    </div>
  </Modal>
  <style>
     
  </style>
  