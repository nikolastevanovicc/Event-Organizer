<script>
    import { FloatingLabelInput, Helper, Modal } from 'flowbite-svelte';
    import { ExclamationCircleOutline } from 'flowbite-svelte-icons';
    import { onMount } from 'svelte';
    import { List, Li, Card, Button, Navbar, NavBrand, NavLi, NavUl, NavHamburger, Input, Label, Heading } from 'flowbite-svelte';
    import { ArrowRightOutline, HomeSolid, SearchOutline, TrashBinSolid, EditSolid, CirclePlusSolid, CashSolid  } from 'flowbite-svelte-icons';
    export let data;


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
            if (user[key] == '' || user[key] == null) {
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
  event.preventDefault(); // Sprečavanje defaultnog submitovanja forme
  // Otvorite modal za potvrdu umesto da odmah submitujete formu
  submitModal = true;
}

function confirmSubmit() {
  if (validateUser(user)) {
    // Ako su sva polja validna, izvršite logiku za submitovanje forme
    alert('Sve je u redu');
    submitModal = false;
  }
}
  
    onMount(async () => {
  const actualId = data.id;
  const response = await fetch('https://event-organizer-dae1b-default-rtdb.europe-west1.firebasedatabase.app/korisnici.json');
  const users = await response.json();
  console.log("users",users);
  console.log("actualId",data);

  if (actualId !== 'add') {
            user = users[actualId] || user;
        }
  console.log("user", user);
});
  </script>
  
  <Heading class = "flex flex-col items-center justify-center ml-10 mt-24 mb-10 "tag="h3">{heading}</Heading>

  <form class = "mr-10 ml-10" on:submit={submitUser}>
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
    </div>
    <div class="mb-6">
      <Label for="zanimanje">Zanimanje</Label>
      <Input type="text" id="zanimanje" bind:value={user.zanimanje} />
    </div>
    <div class="mb-6">
      <Label for="lozinka">Lozinka</Label>
      <Input type="password" id="lozinka" bind:value={user.lozinka} />
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