<script>
    import { FloatingLabelInput, Helper } from 'flowbite-svelte';
    import { List, Li, Card, Button, Navbar, NavBrand, NavLi, NavUl, NavHamburger, Input, Label, Heading, Fileupload  } from 'flowbite-svelte';
    import { onMount } from 'svelte';
    import data from '../../../lib/assets/data.json';
    import { ArrowRightOutline, HomeSolid, SearchOutline, TrashBinSolid, EditSolid, CirclePlusSolid, CashSolid  } from 'flowbite-svelte-icons';
    let value;
    let festivals = [];
    let trenutnaSlika = 1;
    // export let id = undefined;
    let festival;   
  console.log(data.festivali)  

  onMount(async () => {
    Object.values(data.festivali).forEach((value) => {
    Object.values(value).forEach((value2) => {
      festivals.push(value2);
    });
    festivals = festivals;
  });
  festival = festivals[0];
  console.log("festival", festival)
});
</script>
<Heading class = "flex flex-col items-center justify-center ml-10 mt-24 "tag="h3">Kreiranje novog festivala</Heading>
<form class = "ml-10 mr-10">
    <div class="mt-10 ml-5 mr-5 grid gap-6 mb-6 md:grid-cols-2">
      <div>
        <Label for="first_name" class="mb-2">Naziv Festivala</Label>
        <Input type="text" id="first_name" placeholder="" required />
      </div>
      <div>
        <Label for="company" class="mb-2">Tip</Label>
        <Input type="text" id="company" placeholder="" required />
      </div>
      <div>
        <Label for="phone" class="mb-2">Cena</Label>
        <Input type="tel" id="phone" placeholder="123-45-678" pattern={"[0-9]{3}-[0-9]{2}-[0-9]{3}"} required />
      </div>
      <div>
        <Label for="website" class="mb-2">Prevoz</Label>
        <Input type="url" id="website" placeholder="" required />
      </div>
      <div>
        <Label for="visitors" class="mb-2">Maksimalan broj osoba</Label>
        <Input type="number" id="visitors" placeholder="" required />
      </div>
    </div>
    <div class="mb-6 ml-5 mr-5">
      <Label for="email" class="mb-2">Opis</Label>
      <Input size="lg"type="email" id="email" placeholder="john.doe@company.com" required />
    </div>
    <Label class="space-y-2 mb-2 ml-5">
        <span>Upload file</span>
        <Fileupload  bind:value />
      </Label>
      <Label class = "ml-5">File: {value}</Label>
    <Button class = " mt-5 ml-5 mr-5" type="submit">Submit</Button>
  </form>