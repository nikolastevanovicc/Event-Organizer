<script>
    import { FloatingLabelInput, Helper } from 'flowbite-svelte';
    import { List, Li, Card, Button, Navbar, NavBrand, NavLi, NavUl, NavHamburger, Input, Label, Heading, Fileupload  } from 'flowbite-svelte';
    import { onMount } from 'svelte';
    import { ArrowRightOutline, HomeSolid, SearchOutline, TrashBinSolid, EditSolid, CirclePlusSolid, CashSolid  } from 'flowbite-svelte-icons';
    let value;
    let festivals = [];
    let trenutnaSlika = 1;

    let festival = {
    naziv: '',
    tip: '',
    cena: '',
    prevoz: '',
    maksBrojOsoba: '',
    opis: ''
  };  
  
  function validateFestival(festival) {
    for (let key in festival) {
      if (!festival[key]) {
        alert(`Molimo unesite ${key}.`);
        return false;
      }
    }
    return true;
  }

  function submitFestival(event) {
    event.preventDefault();

    if (!validateFestival(festival)) {
      return;
    }

    console.log(festival);
    alert('Svi podaci su pravilno uneseni!');
  }
</script>
<Heading class = "flex flex-col items-center justify-center ml-10 mt-24 "tag="h3">Kreiranje novog festivala</Heading>
<form class = "ml-10 mr-10" on:submit={submitFestival}>
    <div class="mt-10 ml-5 mr-5 grid gap-6 mb-6 md:grid-cols-2">
      <div>
        <Label for="naziv" class="mb-2">Naziv Festivala</Label>
        <Input bind:value={festival.naziv} type="text" id="naziv" placeholder="" required />
      </div>
      <div>
        <Label for="tip" class="mb-2">Tip</Label>
        <Input bind:value={festival.tip} type="text" id="tip" placeholder="" required />
      </div>
      <div>
        <Label for="cena" class="mb-2">Cena</Label>
        <Input bind:value={festival.cena} type="text" id="cena" placeholder="" required />
      </div>
      <div>
        <Label for="prevoz" class="mb-2">Prevoz</Label>
        <Input bind:value={festival.prevoz} type="text" id="prevoz" placeholder="" required />
      </div>
      <div>
        <Label for="maksBrojOsoba" class="mb-2">Maksimalan broj osoba</Label>
        <Input bind:value={festival.maksBrojOsoba} type="number" id="maksBrojOsoba" placeholder="" required />
      </div>
    </div>
    <div class="mb-6 ml-5 mr-5">
      <Label for="opis" class="mb-2">Opis</Label>
      <Input bind:value={festival.opis} size="lg" type="text" id="opis" placeholder="" required />
    </div>
    <Label class="space-y-2 mb-2 ml-5">
        <span>Upload file</span>
        <Fileupload  bind:value />
      </Label>
      <Label class = "ml-5">File: {value}</Label>
    <Button class = " mt-5 ml-5 mr-5" type="submit">Submit</Button>
  </form>