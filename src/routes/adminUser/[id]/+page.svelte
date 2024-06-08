<script>
  import { FloatingLabelInput, Helper, Modal, Label } from 'flowbite-svelte';
  import { List, Li, Card, Heading, Button, Navbar, NavBrand, NavLi, NavUl, NavHamburger, Input, Table, TableBody, TableBodyCell, TableBodyRow, TableHead, TableHeadCell } from 'flowbite-svelte';
  import { onMount } from 'svelte';
  import { ArrowRightOutline, HomeSolid, SearchOutline, TrashBinSolid, EditSolid, CirclePlusSolid, ExclamationCircleOutline } from 'flowbite-svelte-icons';

  const FIREBASE_URL = 'https://event-organizer-dae1b-default-rtdb.europe-west1.firebasedatabase.app/korisnici';
  let formModal = false;

let loginModal = false;
let registerModal = false;
function closeAndNavigate() {
formModal = false;
window.location.href = '../editUser/add';
}
  let deleteModal = false;
  let userIdToDelete = null;
  let users = [];

  function openDeleteModal(userId) {
    userIdToDelete = userId;
    deleteModal = true;
  }

  async function deleteUser() {
    try {
      await fetch(`${FIREBASE_URL}/${userIdToDelete}.json`, {
        method: 'DELETE'
      });

      // Update local users array
      users = users.filter(user => user.id !== userIdToDelete);
      console.log(`Deleted user with ID: ${userIdToDelete}`);
    } catch (error) {
      console.error('Error deleting user:', error);
    } finally {
      deleteModal = false;
    }
  }

  onMount(async () => {
    try {
      const response = await fetch(`${FIREBASE_URL}.json`);
      const data = await response.json();
      users = Object.entries(data).map(([id, userInfo]) => ({ id, ...userInfo }));
      console.log('Fetched users:', users);
    } catch (error) {
      console.error('Error fetching users:', error);
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
<Heading class="flex flex-col items-center justify-center ml-10 mt-24 mb-20" tag="h3">Prikaz svih korisnika</Heading>
<Table>
  <TableHead>
    <TableHeadCell>ID</TableHeadCell>
    <TableHeadCell>Username</TableHeadCell>
    <TableHeadCell>Ime</TableHeadCell>
    <TableHeadCell>Prezime</TableHeadCell>
    <TableHeadCell>
      <span class="sr-only">Edit</span>
    </TableHeadCell>
    <TableHeadCell>
      <span class="sr-only">Delete</span>
    </TableHeadCell>
  </TableHead>
  <TableBody class="divide-y">
    {#each users as user}
      <TableBodyRow>
        <TableBodyCell>{user.id}</TableBodyCell>
        <TableBodyCell>{user.korisnickoIme}</TableBodyCell>
        <TableBodyCell>{user.ime}</TableBodyCell>
        <TableBodyCell>{user.prezime}</TableBodyCell>
        <TableBodyCell>
          <a href={`/editUser/${user.id}`} class="font-medium text-primary-600 hover:underline dark:text-primary-500">Edit</a>
        </TableBodyCell>
        <TableBodyCell>
          <button class="font-medium text-primary-600 hover:underline dark:text-primary-500" on:click={() => openDeleteModal(user.id)}>Delete</button>
        </TableBodyCell>
      </TableBodyRow>
    {/each}
  </TableBody>
</Table>

<Modal bind:open={deleteModal} size="xs" autoclose>
  <div class="text-center">
    <ExclamationCircleOutline class="mx-auto mb-4 text-gray-400 w-12 h-12 dark:text-gray-200" />
    <h3 class="mb-5 text-lg font-normal text-gray-500 dark:text-gray-400">Are you sure you want to delete this user?</h3>
    <Button color="red" class="me-2" on:click={deleteUser}>Yes, I'm sure</Button>
    <Button color="alternative" on:click={() => deleteModal = false}>No, cancel</Button>
  </div>
</Modal>
