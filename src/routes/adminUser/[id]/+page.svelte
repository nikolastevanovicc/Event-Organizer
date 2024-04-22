<script>
        import { FloatingLabelInput, Helper } from 'flowbite-svelte';
    import { List, Li, Card,Heading, Button, Navbar, NavBrand, NavLi, NavUl, NavHamburger, Input, Table, TableBody, TableBodyCell, TableBodyRow, TableHead, TableHeadCell } from 'flowbite-svelte';
    import { onMount } from 'svelte';
    import data from '../../../lib/assets/data.json';
    import { ArrowRightOutline, HomeSolid, SearchOutline, TrashBinSolid, EditSolid, CirclePlusSolid  } from 'flowbite-svelte-icons';
</script>

  <Heading class = "flex flex-col items-center justify-center ml-10 mt-24 mb-20 "tag="h3">Prikaz svih korisnika</Heading>
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
      <TableBodyRow>
        <TableBodyCell>111</TableBodyCell>
        <TableBodyCell>nikola1</TableBodyCell>
        <TableBodyCell>Nikola</TableBodyCell>
        <TableBodyCell>Stevanovic</TableBodyCell>
        <TableBodyCell>
          <a href="/editUser/1" class="font-medium text-primary-600 hover:underline dark:text-primary-500">Edit</a>
        </TableBodyCell>
        <TableBodyCell>
            <a href="/tables" class="font-medium text-primary-600 hover:underline dark:text-primary-500">Delete</a>
          </TableBodyCell>
      </TableBodyRow>
      <TableBodyRow>
        <TableBodyCell>123</TableBodyCell>
        <TableBodyCell>Bosko1</TableBodyCell>
        <TableBodyCell>Bosko</TableBodyCell>
        <TableBodyCell>Mitrovic</TableBodyCell>
        <TableBodyCell>
          <a href="/editUser/1" class="font-medium text-primary-600 hover:underline dark:text-primary-500">Edit</a>
        </TableBodyCell>
        <TableBodyCell>
            <a href="/tables" class="font-medium text-primary-600 hover:underline dark:text-primary-500">Delete</a>
          </TableBodyCell>
      </TableBodyRow>
    </TableBody>
  </Table>