<script>
    import ChevronDownSize20 from "./Chevron.svelte";
    export let content =
      "Answer the frequently asked question in a simple sentence, a longish paragraph, or even in a list.";
    export let title = "Title";
    export let state = "open";
    let className = "";
    export { className as class };
    const variantsClassName = "state-" + state;
  </script>
  
  <div
    class="{'accordion-item-state-closed ' + className + ' ' + variantsClassName}"
  >
    <div class="accordion-title">
      <div class="title">{title}</div>
      <ChevronDownSize20
        size="20"
        class="chevron-down-instance"
      ></ChevronDownSize20>
    </div>
  </div>
  
  <style>
    .accordion-item-state-closed,
    .accordion-item-state-closed * {
      box-sizing: border-box;
    }
    .accordion-item-state-closed {
      background: var(--var-sds-color-background-default-secondary, #f5f5f5);
      border-radius: var(--var-sds-size-radius-200, 8px);
      border-style: solid;
      border-color: var(--var-sds-color-border-default-default, #d9d9d9);
      border-width: 1px;
      padding: var(--var-sds-size-padding-lg, 16px);
      display: flex;
      flex-direction: row;
      gap: 0px;
      align-items: center;
      justify-content: flex-start;
      position: relative;
    }
    .accordion-item-state-closed .accordion-title {
      display: flex;
      flex-direction: row;
      gap: var(--var-sds-size-space-200, 8px);
      align-items: center;
      justify-content: flex-start;
      flex: 1;
      position: relative;
    }
    .accordion-item-state-closed .title {
      color: var(--var-sds-color-text-default-default, #1e1e1e);
      text-align: left;
      font-family: var(--body-strong-font-family, "Inter-SemiBold", sans-serif);
      font-size: var(--body-strong-font-size, 16px);
      line-height: var(--body-strong-line-height, 140%);
      font-weight: var(--body-strong-font-weight, 600);
      position: relative;
      flex: 1;
    }
  </style>
  