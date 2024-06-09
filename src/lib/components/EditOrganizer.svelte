<script>
  import { FloatingLabelInput, Helper, Modal } from 'flowbite-svelte';
  import { List, Li, Card, Button, Navbar, NavBrand, NavLi, NavUl, NavHamburger, Input, Label, Heading, Fileupload } from 'flowbite-svelte';
  import { onMount } from 'svelte';
  import { ArrowRightOutline, HomeSolid, SearchOutline, TrashBinSolid, EditSolid, CirclePlusSolid, CashSolid  } from 'flowbite-svelte-icons';
  import { ExclamationCircleOutline } from 'flowbite-svelte-icons';

  export let organizerId = undefined;
  let formModal = false;
  let loginModal = false;
  let registerModal = false;

  function closeAndNavigate() {
    formModal = false;
    window.location.href = '../editUser/add';
  }

  let festivals = [];
  let organizer = {
    adresa: '',
    email: '',
    festivali: '',
    godinaOsnivanja: '',
    kontaktTelefon: '',
    logo: '',
    naziv: ''
  };
  let errors = {
    adresa: '',
    email: '',
    godinaOsnivanja: '',
    kontaktTelefon: '',
    logo: '',
    naziv: ''
  };
  let deleteModal = false;
  let submitModal = false;
  let selectedFestival = null;
  let logoFile = null;

  onMount(async () => {
    const response = await fetch('https://event-organizer-dae1b-default-rtdb.europe-west1.firebasedatabase.app/organizatoriFestivala.json');
    const organizatoriFestivala = await response.json();

    if (organizatoriFestivala[organizerId]) {
      organizer = organizatoriFestivala[organizerId];
      if (organizer.festivali) {
        const responseFestivals = await fetch('https://event-organizer-dae1b-default-rtdb.europe-west1.firebasedatabase.app/festivali.json');
        const festivali = await responseFestivals.json();

        festivals = Object.entries(festivali[organizer.festivali] || {}).map(([id, value]) => ({ id, ...value }));
      }
    }
  });

  function confirmDelete(festival) {
    selectedFestival = festival;
    deleteModal = true;
  }

  async function deleteFestival() {
    if (!selectedFestival) return;

    const festivalId = selectedFestival.id;
    const organizerFestivaliRef = `https://event-organizer-dae1b-default-rtdb.europe-west1.firebasedatabase.app/festivali/${organizer.festivali}/${festivalId}.json`;

    await fetch(organizerFestivaliRef, { method: 'DELETE' });
    festivals = festivals.filter(festival => festival.id !== festivalId);
    deleteModal = false;
    selectedFestival = null;
  }

  function submitForm() {
    if (validateOrganizer(organizer)) {
      submitModal = true;
    }
  }

  function validateOrganizer(organizer) {
    let isValid = true;
    const currentYear = new Date().getFullYear();

    for (let key in organizer) {
      if (!organizer[key]) {
        errors[key] = `Molimo unesite ${key}.`;
        isValid = false;
      } else {
        errors[key] = '';
      }

      if (key === 'email' && !/^\S+@\S+\.\S+$/.test(organizer[key])) {
        errors[key] = 'Molimo unesite validan email.';
        isValid = false;
      }

      if (key === 'kontaktTelefon' && !/^(\+?\d{1,3}[-.\s]?)?(\(?\d{1,4}\)?[-.\s]?)?[\d\s/.-]{5,}$/i.test(organizer[key])) {
        errors[key] = 'Molimo unesite validan broj telefona.';
        isValid = false;
      }

      if (key === 'godinaOsnivanja' && (!/^\d{4}$/.test(organizer[key]) || organizer[key] < 1900 || organizer[key] > currentYear)) {
        errors[key] = `Molimo unesite validnu godinu između 1900 i ${currentYear}.`;
        isValid = false;
      }
    }
    return isValid;
  }

  async function submitOrganizer(event) {
    event.preventDefault(); 

    if (!validateOrganizer(organizer)) {
      return;
    }

    // Handle the logo file upload if it exists
    if (logoFile) {
      // Here, you need to upload the logoFile to your server or storage (like Firebase Storage)
      // This is a placeholder for the upload logic
      // After uploading, you should get the URL of the uploaded file and set it to organizer.logo
      // For example:
      // const logoUrl = await uploadLogoFile(logoFile);
      // organizer.logo = logoUrl;
    }

    const organizerRef = `https://event-organizer-dae1b-default-rtdb.europe-west1.firebasedatabase.app/organizatoriFestivala/${organizerId}.json`;
    await fetch(organizerRef, {
      method: 'PUT',
      headers: {
        'Content-Type': 'application/json'
      },
      body: JSON.stringify(organizer)
    });

    alert('Podaci su uspešno sačuvani!');
    submitModal = false;
  }

  function handleFileChange(event) {
    const file = event.target.files[0];
    if (file) {
      logoFile = file;
    }
  }
</script>

<Navbar class="fixed w-full top-0 left-0 bg-[#e5e7eb] bg-opacity-95">
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

<Heading class="flex flex-col items-center justify-center ml-10 mt-24" tag="h3">Kreiranje/izmena organizatora festivala</Heading>
<form class="mr-10 ml-10" on:submit={submitOrganizer}>
  <div class="mt-10 ml-5 mr-5 grid gap-6 mb-6 md:grid-cols-2">
    <div>
      <Label for="naziv" class="mb-2">Naziv Organizatora</Label>
      <Input bind:value={organizer.naziv} type="text" id="naziv" class={errors.naziv ? 'border-red-500' : ''} placeholder="" required />
      {#if errors.naziv}<span class="text-red-500">{errors.naziv}</span>{/if}
    </div>
    <div>
      <Label for="adresa" class="mb-2">Adresa</Label>
      <Input bind:value={organizer.adresa} type="text" id="adresa" class={errors.adresa ? 'border-red-500' : ''} placeholder="" required />
      {#if errors.adresa}<span class="text-red-500">{errors.adresa}</span>{/if}
    </div>
    <div>
      <Label for="godinaOsnivanja" class="mb-2">Godina Osnivanja</Label>
      <Input bind:value={organizer.godinaOsnivanja} type="text" id="godinaOsnivanja" class={errors.godinaOsnivanja ? 'border-red-500' : ''} placeholder="" required />
      {#if errors.godinaOsnivanja}<span class="text-red-500">{errors.godinaOsnivanja}</span>{/if}
    </div>
    <div>
      <Label for="email" class="mb-2">Email</Label>
      <Input bind:value={organizer.email} type="email" id="email" class={errors.email ? 'border-red-500' : ''} placeholder="" required />
      {#if errors.email}<span class="text-red-500">{errors.email}</span>{/if}
    </div>
    <div>
      <Label for="kontaktTelefon" class="mb-2">Kontakt Telefon</Label>
      <Input bind:value={organizer.kontaktTelefon} type="tel" id="kontaktTelefon" class={errors.kontaktTelefon ? 'border-red-500' : ''} placeholder="" required />
      {#if errors.kontaktTelefon}<span class="text-red-500">{errors.kontaktTelefon}</span>{/if}
    </div>
    <div>
      <Label for="logo" class="mb-2">Logo URL</Label>
      <Input bind:value={organizer.logo} type="text" id="logo" class={errors.logo ? 'border-red-500' : ''} placeholder="https://example.com/logo.png" required />
      {#if errors.logo}<span class="text-red-500">{errors.logo}</span>{/if}
    </div>
    <Button class="mt-5" type="button" on:click={submitForm}>Submit</Button>
  </div>
</form>

<Modal bind:open={submitModal} size="xs" autoclose>
  <div class="text-center">
    <ExclamationCircleOutline class="mx-auto mb-4 text-gray-400 w-12 h-12 dark:text-gray-200" />
    <h3 class="mb-5 text-lg font-normal text-gray-500 dark:text-gray-400">Are you sure you want to submit this form?</h3>
    <Button color="red" class="me-2" on:click={submitOrganizer}>Yes, I'm sure</Button>
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
