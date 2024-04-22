<script>
    import { FloatingLabelInput, Helper } from 'flowbite-svelte';
    import { List, Li, Card, Button, Navbar, NavBrand, NavLi, NavUl, NavHamburger, Input, Label, Heading } from 'flowbite-svelte';
    import { onMount } from 'svelte';
    import dataa from '$lib/assets/data.json';
    import { ArrowRightOutline, HomeSolid, SearchOutline, TrashBinSolid, EditSolid, CirclePlusSolid, CashSolid  } from 'flowbite-svelte-icons';
    import EditOrganizer from '$lib/components/EditOrganizer.svelte';
    let festivals = [];
    let trenutnaSlika = 1;
    // export let id = undefined;
    let festival;   
    let source;
    export let data;
    console.log(data)
  onMount(async () => {
    Object.values(dataa.festivali).forEach((value) => {
    Object.values(value).forEach((value2) => {
      festivals.push(value2);
    });
    festivals = festivals;
  });
  festival = festivals[0];
  console.log("festival", festival)
});
</script>
  
<EditOrganizer organizerId={data.id} />