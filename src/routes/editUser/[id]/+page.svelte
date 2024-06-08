<script>
  import { FloatingLabelInput, Helper, Modal } from 'flowbite-svelte';
  import { ExclamationCircleOutline } from 'flowbite-svelte-icons';
  import { onMount } from 'svelte';
  import { List, Li, Card, Button, Navbar, NavBrand, NavLi, NavUl, NavHamburger, Input, Label, Heading } from 'flowbite-svelte';
  import { ArrowRightOutline, HomeSolid, SearchOutline, TrashBinSolid, EditSolid, CirclePlusSolid, CashSolid  } from 'flowbite-svelte-icons';
  import { json } from '@sveltejs/kit';
  export let data;
  let formModal = false;

  let loginModal = false;
  let registerModal = false;
function closeAndNavigate() {
formModal = false;
window.location.href = '../editUser/add';
}

  const FIREBASE_URL = 'https://event-organizer-dae1b-default-rtdb.europe-west1.firebasedatabase.app/korisnici';

  let user = {
      adresa: '',
      datumRodjenja: '',
      email: '',
      ime: '',
      korisnickoIme: '',
      lozinka: '',
      prezime: '',
      telefon: '',
      zanimanje: ''
  };

  function validateUser(user) {
      for (let key in user) {
          if (user[key] === '' || user[key] === null) {
              alert(`Molimo unesite ${key}.`);
              return false;
          }
      }
      return true;
  }

  let heading;

  if (data.id === 'add') {
      heading = 'Registracija';
  } else {
      heading = 'Izmena informacija o korisniku';
  }

  let submitModal = false;

  function submitUser(event) {
      event.preventDefault();
      if (validateUser(user)) {
          submitModal = true; // Open modal for confirmation
      }
  }

  function confirmSubmit() {
      registerOrUpdateUser();
      submitModal = false; // Close modal
  }

  async function registerOrUpdateUser() {
      try {
          let method = 'POST';
          let endpoint = `${FIREBASE_URL}.json`;

          if (data.id !== 'add') {
              method = 'PATCH';
              endpoint = `${FIREBASE_URL}/${data.id}.json`;
          }

          const response = await fetch(endpoint, {
              method: method,
              headers: {
                  'Content-Type': 'application/json'
              },
              body: JSON.stringify(user)
          });

          if (!response.ok) {
              throw new Error(`HTTP error! status: ${response.status}`);
          }

          const responseData = await response.json();
          alert('Operacija uspješna!');
      } catch (error) {
          console.error('Error:', error);
          alert('Došlo je do greške prilikom operacije.');
      }
  }

  onMount(async () => {
      if (data.id !== 'add') {
          const response = await fetch(`${FIREBASE_URL}/${data.id}.json`);
          if (response.ok) {
              const userData = await response.json();
              if (userData) {
                  user = { ...userData };
              }
          } else {
              console.error('Error fetching user data:', response.statusText);
          }
      }
  });
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
<Heading class="flex flex-col items-center justify-center ml-10 mt-24 mb-10" tag="h3">{heading}</Heading>

<form class="mr-10 ml-10" on:submit|preventDefault={submitUser}>
  <div class="grid gap-6 mb-6 md:grid-cols-2">
      <div>
          <Label for="korisnickoIme">Korisničko ime</Label>
          <Input type="text" id="korisnickoIme" bind:value={user.korisnickoIme} />
      </div>
      <div>
          <Label for="ime">Ime</Label>
          <Input type="text" id="ime" bind:value={user.ime} />  
      </div>
      <div>
          <Label for="prezime">Prezime</Label>
          <Input type="text" id="prezime" bind:value={user.prezime} />
      </div>
      <div>
          <Label for="adresa">Adresa</Label>
          <Input type="text" id="adresa" bind:value={user.adresa} />
      </div>
      <div>
          <Label for="telefon">Telefon</Label>
          <Input type="tel" id="telefon" bind:value={user.telefon} />
      </div>
      <div>
          <Label for="email">Email</Label>
          <Input type="email" id="email" bind:value={user.email} />
      </div>
      <div>
          <Label for="datumRodjenja">Datum rođenja</Label>
          <Input type="date" id="datumRodjenja" bind:value={user.datumRodjenja} />
      </div>
      <div>
          <Label for="zanimanje">Zanimanje</Label>
          <Input type="text" id="zanimanje" bind:value={user.zanimanje} />
      </div>
      <div>
          <Label for="lozinka">Lozinka</Label>
          <Input type="password" id="lozinka" bind:value={user.lozinka} />
      </div>
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
