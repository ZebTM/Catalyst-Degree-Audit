<script>
    export let size = "48";
    let className = "";
    export { className as class };
    const variantsClassName = "size-" + size;
  </script>
  
  <img
    class="{'chevron-up-size-20 ' + className + ' ' + variantsClassName}"
    src="chevron-up-size-20.svg"
  />
  
  <style>
    .chevron-up-size-20,
    .chevron-up-size-20 * {
      box-sizing: border-box;
    }
    .chevron-up-size-20 {
      flex-shrink: 0;
      width: 20px;
      height: 20px;
      position: relative;
      overflow: visible;
    }
  </style>
  