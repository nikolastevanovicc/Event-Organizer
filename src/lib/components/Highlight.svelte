<!-- Highlight.svelte -->
<script>
    export let text = "";
    export let query = "";
    
    function getHighlightedText(text, query) {
      if (!query) return [{ text, highlight: false }];
      const regex = new RegExp(`(${query})`, 'gi');
      const parts = text.split(regex);
      return parts.map(part => ({ text: part, highlight: regex.test(part) }));
    }
  </script>
  
  <span>
    {#each getHighlightedText(text, query) as part}
      <span class:highlight={part.highlight}>{part.text}</span>
    {/each}
  </span>
  
  <style>
    .highlight {
      background-color: rgb(220, 92, 12);
    }
  </style>
  