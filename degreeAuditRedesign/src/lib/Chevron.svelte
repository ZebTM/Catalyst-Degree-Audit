<script>
  let className = "";
  export { className as class };
</script>

<svg 
  class={`chevron ${className}`} 
  width="20" 
  height="20" 
  viewBox="0 0 20 20" 
  fill="none" 
  xmlns="http://www.w3.org/2000/svg"
  aria-hidden="true"
>
  <path 
    d="M5 7.5L10 12.5L15 7.5" 
    stroke="#888888" 
    stroke-width="2" 
    stroke-linecap="round" 
    stroke-linejoin="round"
  />
</svg>

<style>
  .chevron {
    transition: transform 0.3s ease;
  }
</style>
  