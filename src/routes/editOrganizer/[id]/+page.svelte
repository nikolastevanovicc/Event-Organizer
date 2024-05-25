<script>
    import { FloatingLabelInput, Helper } from 'flowbite-svelte';
    import { List, Li, Card, Button, Navbar, NavBrand, NavLi, NavUl, NavHamburger, Input, Label, Heading } from 'flowbite-svelte';
    import { onMount } from 'svelte';
    import { ArrowRightOutline, HomeSolid, SearchOutline, TrashBinSolid, EditSolid, CirclePlusSolid, CashSolid  } from 'flowbite-svelte-icons';
    import EditOrganizer from '$lib/components/EditOrganizer.svelte';
    export let data;
    let festivals = [];


let organizer;
onMount(async () => {
  const actualId = data.id;
  const response = await fetch('https://event-organizer-dae1b-default-rtdb.europe-west1.firebasedatabase.app/organizatoriFestivala.json');
  const organizatoriFestivala = await response.json();

  Object.entries(organizatoriFestivala).forEach(([key, value]) => {
    if (key == actualId) {
      organizer = value;
    }
  });

  if (organizer && organizer.festivali) {
    const responseFestivals = await fetch('https://event-organizer-dae1b-default-rtdb.europe-west1.firebasedatabase.app/festivali.json');
    const festivali = await responseFestivals.json();

    Object.entries(festivali).forEach(([key, value]) => {
      if (key == organizer.festivali) {
        Object.entries(value).forEach(([key2, value2]) => {
          const festival = { id: key2, ...value2 };
          festivals = [...festivals, festival]; // Update reactively
        });
      }
    });
  }
});
</script>
  
<EditOrganizer organizerId={data.id} />