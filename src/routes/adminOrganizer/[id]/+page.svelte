<script>
  import { FloatingLabelInput, Helper,Label } from 'flowbite-svelte';
  import { List, Li, Card, Button, Navbar, NavBrand, NavLi, NavUl, NavHamburger, Input } from 'flowbite-svelte';
  import { onMount } from 'svelte';
  import { ArrowRightOutline, HomeSolid, SearchOutline, TrashBinSolid, EditSolid, CirclePlusSolid  } from 'flowbite-svelte-icons';
  import { Modal } from 'flowbite-svelte';
  import { ExclamationCircleOutline } from 'flowbite-svelte-icons';
  let formModal = false;

let loginModal = false;
let registerModal = false;
function closeAndNavigate() {
formModal = false;
window.location.href = '../editUser/add';
}
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

  async function deleteOrganizer() {
    try {
      const response = await fetch(`https://event-organizer-dae1b-default-rtdb.europe-west1.firebasedatabase.app/organizatoriFestivala/${selectedOrganizer.id}.json`, {
        method: 'DELETE'
      });

      if (!response.ok) {
        throw new Error(`Error: ${response.statusText}`);
      }

      organizers = organizers.filter(organizer => organizer.id !== selectedOrganizer.id);
      console.log(`Deleted organizer: ${selectedOrganizer.id}`);
    } catch (error) {
      console.error('Error deleting organizer:', error);
    } finally {
      popupModal = false;
    }
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
<div class="flex justify-center mt-20">
<Button class="mt-5">
  <CirclePlusSolid class="mr-1 w-3.5 h-3.5 ms-2 text-white" />
  <a href="/editOrganizer/add">Add Organizer</a>
</Button>
</div>

<div class="flex justify-center items-center my-14">
<div class="grid md:grid-cols-4 sm:grid-cols-2 gap-4 w-[80%] mt-14">
  {#each organizers as organizer (organizer.id)}
    <Card img={organizer.logo}>
      <h5 class="mb-2 text-2xl font-bold tracking-tight text-gray-900 dark:text-white">{organizer.naziv}</h5>
      <div class="flex items-center">
        <HomeSolid color="green"></HomeSolid>
        <p class="font-bold ml-1">{organizer.adresa}</p>
      </div>
      <Button class="mt-5">
        <EditSolid class="w-3.5 h-3.5 ms-2 text-white" />
        <a href={`/editOrganizer/${organizer.id}`}>Edit</a>
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
  <h3 class="mb-5 text-lg font-normal text-gray-500 dark:text-gray-400">Are you sure you want to delete this organizer?</h3>
  <Button color="red" class="me-2" on:click={deleteOrganizer}>Yes, I'm sure</Button>
  <Button color="alternative" on:click={() => popupModal = false}>No, cancel</Button>
</div>
</Modal>
