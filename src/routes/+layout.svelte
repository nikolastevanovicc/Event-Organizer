<script>
    import { FloatingLabelInput, Helper, Button, Modal, Label, Input, Checkbox  } from 'flowbite-svelte';
  import { List, Li, Card, Navbar, NavBrand, NavLi, NavUl, NavHamburger,P } from 'flowbite-svelte';
  import { onMount } from 'svelte';
  import { ArrowRightOutline, HomeSolid, SearchOutline, CashSolid, MailBoxSolid,PhoneSolid  } from 'flowbite-svelte-icons';
    import "../app.css";
    let formModal = false;

  let loginModal = false;
  let registerModal = false;
  function closeAndNavigate() {
    formModal = false;
    window.location.href = '../editUser/add';
  }
  </script>
  
  <slot />