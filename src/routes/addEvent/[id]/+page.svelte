<script>
  import { FloatingLabelInput, Helper, Modal, NavBrand, NavHamburger, NavLi, NavUl, Navbar } from 'flowbite-svelte';
  import { ExclamationCircleOutline } from 'flowbite-svelte-icons';
  import { onMount } from 'svelte';
  import { Input, Label, Heading, Button } from 'flowbite-svelte';
  import { json } from '@sveltejs/kit';
  let formModal = false;

let loginModal = false;
let registerModal = false;
function closeAndNavigate() {
formModal = false;
window.location.href = '../editUser/add';
}
  export let data;
  let organizers = [];
  let organizer = {
      adresa: '',
      email: '',
      festivali: '',
      godinaOsnivanja: '',
      kontaktTelefon: '',
      logo: '',
      naziv: ''
  };
  const FIREBASE_URL = 'https://event-organizer-dae1b-default-rtdb.europe-west1.firebasedatabase.app';

  let festival = {
      naziv: '',
      tip: '',
      cena: '',
      prevoz: '',
      maksBrojOsoba: '',
      opis: '',
      slike: [] // Novo polje za URL-ove slika
  };

  let imageUrls = ['']; // Promenljiva za čuvanje URL-ova slika

  onMount(async () => {
      const response = await fetch(`${FIREBASE_URL}/organizatoriFestivala.json`);
      const organizatoriFestivala = await response.json();

      organizers = Object.entries(organizatoriFestivala).map(([id, value]) => ({ id, ...value }));

      // Pretpostavljamo da je `data` ID organizatora koji tražite
      organizer = organizers.find(organizer => organizer.id === data.id);

      console.log("organizer", organizer);
  });

  function validateFestival(festival) {
      for (let key in festival) {
          if (festival[key] === '' || festival[key] === null) {
              alert(`Molimo unesite ${key}.`);
              return false;
          }
      }
      return true;
  }

  let submitModal = false;

  function submitFestival(event) {
      event.preventDefault();
      if (validateFestival(festival)) {
          submitModal = true; // Open modal for confirmation
      }
  }

  function confirmSubmit() {
      festival.slike = imageUrls.filter(url => url !== '');
      registerOrUpdateFestival();
      submitModal = false; // Close modal
  }

  async function registerOrUpdateFestival() {
      // Dodavanje festivala na tačnu lokaciju pod organizatorom
      const response = await fetch(`${FIREBASE_URL}/festivali/${organizer.festivali}.json`, {
          method: 'POST',
          headers: {
              'Content-Type': 'application/json'
          },
          body: JSON.stringify(festival)
      });

      const newFestival = await response.json();
      const newFestivalId = newFestival.name;

                  

      alert('Festival uspešno dodat!');
  }

  function addImageUrlField() {
      imageUrls.push('');
  }

  function removeImageUrlField(index) {
      imageUrls.splice(index, 1);
  }
</script>
<Navbar class = "fixed w-full top-0 left-0 bg-[#e5e7eb] bg-opacity-95">
  <NavBrand href="/">
    <img src="../src/lib/assets/logo-no-background.png" class="me-3 h-7 sm:h-14" alt="Event Organizers" />
    <span class="self-center whitespace-nowrap text-xl font-semibold dark:text-white">Event Organizers</span>
  </NavBrand>
  <div class="flex md:order-1">
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
<Heading class="flex flex-col items-center justify-center ml-10 mt-24 mb-10" tag="h3">Kreiranje novog festivala</Heading>

<form class="mr-10 ml-10" on:submit|preventDefault={submitFestival}>
  <div class="grid gap-6 mb-6 md:grid-cols-2">
      <div>
          <Label for="naziv">Naziv Festivala</Label>
          <Input type="text" id="naziv" bind:value={festival.naziv} required />
      </div>
      <div>
          <Label for="tip">Tip</Label>
          <Input type="text" id="tip" bind:value={festival.tip} required />
      </div>
      <div>
          <Label for="cena">Cena</Label>
          <Input type="text" id="cena" bind:value={festival.cena} required />
      </div>
      <div>
          <Label for="prevoz">Prevoz</Label>
          <Input type="text" id="prevoz" bind:value={festival.prevoz} required />
      </div>
      <div>
          <Label for="maksBrojOsoba">Maksimalan broj osoba</Label>
          <Input type="number" id="maksBrojOsoba" bind:value={festival.maksBrojOsoba} required />
      </div>
  </div>
  <div class="mb-6">
      <Label for="opis">Opis</Label>
      <Input type="text" id="opis" bind:value={festival.opis} required />
  </div>
  <div class="mb-6">
      <Label for="slike">Slike</Label>
      {#each imageUrls as imageUrl, index}
          <div class="flex mb-2">
              <Input type="text" bind:value={imageUrl} placeholder="URL slike" class="mr-2" required />
              <Button on:click={() => removeImageUrlField(index)} type="button">Remove</Button>
          </div>
      {/each}
      <Button on:click={addImageUrlField} type="button">Add another image</Button>
  </div>


  <Button type="submit">Submit</Button>
</form>

<Modal bind:open={submitModal} size="xs" autoclose>
  <div class="text-center">
      <ExclamationCircleOutline class="mx-auto mb-4 text-gray-400 w-12 h-12 dark:text-gray-200" />
      <h3 class="mb-5 text-lg font-normal text-gray-500 dark:text-gray-400">Are you sure you want to submit this form?</h3>
      <Button on:click={confirmSubmit}>Yes, submit</Button>
      <Button on:click={() => submitModal = false}>No, go back</Button>
  </div>
</Modal>
