<script>
  import { FloatingLabelInput, Helper, Modal } from 'flowbite-svelte';
  import { List, Li, Card, Heading, Button, Navbar, NavBrand, NavLi, NavUl, NavHamburger, Input, Table, TableBody, TableBodyCell, TableBodyRow, TableHead, TableHeadCell } from 'flowbite-svelte';
  import { onMount } from 'svelte';
  import { ArrowRightOutline, HomeSolid, SearchOutline, TrashBinSolid, EditSolid, CirclePlusSolid, ExclamationCircleOutline } from 'flowbite-svelte-icons';

  let deleteModal = false;
  let userIdToDelete = null;
  let users = [];

  function openDeleteModal(userId) {
    userIdToDelete = userId;
    deleteModal = true;
  }

  function deleteUser() {
    // Implementacija brisanja korisnika
    console.log(`Deleting user with ID: ${userIdToDelete}`);
    deleteModal = false;
  }

  onMount(async () => {
    try {
      const response = await fetch('https://event-organizer-dae1b-default-rtdb.europe-west1.firebasedatabase.app/korisnici.json');
      const data = await response.json();
      users = Object.entries(data).map(([id, userInfo]) => ({ id, ...userInfo }));
      console.log('Fetched users:', users);
    } catch (error) {
      console.error('Error fetching users:', error);
    }
  });
</script>

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
