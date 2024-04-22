<script>
       import { FloatingLabelInput, Helper } from 'flowbite-svelte';
    import { List, Li, Card, Button, Navbar, NavBrand, NavLi, NavUl, NavHamburger, Input, Label, Heading } from 'flowbite-svelte';
    import { ArrowRightOutline, HomeSolid, SearchOutline, TrashBinSolid, EditSolid, CirclePlusSolid, CashSolid  } from 'flowbite-svelte-icons';
  </script>
  
  <Heading class = "flex flex-col items-center justify-center ml-10 mt-24 mb-10 "tag="h3">Izmena informacija o korisniku</Heading>


  <form class = "mr-10 ml-10">
    <div class="grid gap-6 mb-6 md:grid-cols-2">
        <div>
            <Label for="" class="mb-2">Korisnicko ime</Label>
            <Input type="text" id="" placeholder="John" required />
          </div>
      <div>
        <Label for="first_name" class="mb-2">Ime</Label>
        <Input type="text" id="first_name" placeholder="John" required />
      </div>
      <div>
        <Label for="last_name" class="mb-2">Prezime</Label>
        <Input type="text" id="last_name" placeholder="Doe" required />
      </div>
      <div>
        <Label for="company" class="mb-2">Adresa</Label>
        <Input type="text" id="company" placeholder="" required />
      </div>
      <div>
        <Label for="phone" class="mb-2">Broj telefona</Label>
        <Input type="tel" id="phone" placeholder="123-45-678" pattern={"[0-9]{3}-[0-9]{2}-[0-9]{3}"} required />
      </div>
      <div>
        <Label for="website" class="mb-2">Email adresa</Label>
        <Input type="email" id="website" placeholder="" required />
      </div>
      <div>
        <Label for="visitors" class="mb-2">Datum rodjenja</Label>
        <Input type="text" id="visitors" placeholder="" required />
      </div>
    </div>
    <div class="mb-6">
      <Label for="email" class="mb-2">Zanimanje</Label>
      <Input type="text" id="email" placeholder="" required />
    </div>
    <div class="mb-6">
      <Label for="password" class="mb-2">Lozinka</Label>
      <Input type="password" id="password" placeholder="•••••••••" required />
    </div>
    <div class="mb-6">
      <Label for="confirm_password" class="mb-2">Potvrdite lozinku</Label>
      <Input type="password" id="confirm_password" placeholder="•••••••••" required />
    </div>
    <Button type="submit">Submit</Button>
  </form>