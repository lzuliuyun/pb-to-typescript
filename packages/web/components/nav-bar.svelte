<script>
  export let current = 0;
</script>


<header class="header">
  <a href="/" class={current === 0 ? "active": ""}>PB Converter</a>
  <a href="/mock/" class={current === 1 ? "active": ""}>Mock</a>
  <a href="/ast/" class={current === 2 ? "active": ""}>AST</a>
  <a href="http://www.jsontots.com/" target="_blank">JSON Converter</a>
  <a class="github-link" href="https://github.com/brandonxiang/pb-to-typescript">Github</a>
</header>