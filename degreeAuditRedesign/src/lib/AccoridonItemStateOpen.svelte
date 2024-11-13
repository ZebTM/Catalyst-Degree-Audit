<script>
    import ChevronUpSize20 from "./Chevron.svelte";
    export let content =
      "Answer the frequently asked question in a simple sentence, a longish paragraph, or even in a list.";
    export let title = "Title";
    export let state = "open";
    let className = "";
    export { className as class };
    const variantsClassName = "state-" + state;
  </script>
  
  <div
    class="{'accordion-item-state-open ' + className + ' ' + variantsClassName}"
  >
    <div class="accordion-title">
      <div class="title">{title}</div>
      <ChevronUpSize20 size="20" class="chevron-up-instance"></ChevronUpSize20>
    </div>
    <div class="accordion-content">
      <div class="body">{content}</div>
    </div>
  </div>
  
  <style>
    .accordion-item-state-open,
    .accordion-item-state-open * {
      box-sizing: border-box;
    }
    .accordion-item-state-open {
      background: var(--var-sds-color-background-default-default, #ffffff);
      border-radius: var(--var-sds-size-radius-200, 8px);
      border-style: solid;
      border-color: var(--var-sds-color-border-default-default, #d9d9d9);
      border-width: 1px;
      padding: var(--var-sds-size-padding-lg, 16px);
      display: flex;
      flex-direction: column;
      gap: var(--var-sds-size-space-200, 8px);
      align-items: flex-start;
      justify-content: flex-start;
      position: relative;
    }
    .accordion-item-state-open .accordion-title {
      display: flex;
      flex-direction: row;
      gap: var(--var-sds-size-space-200, 8px);
      align-items: center;
      justify-content: flex-start;
      align-self: stretch;
      flex-shrink: 0;
      position: relative;
    }
    .accordion-item-state-open .title {
      color: var(--var-sds-color-text-default-default, #1e1e1e);
      text-align: left;
      font-family: var(--body-strong-font-family, "Inter-SemiBold", sans-serif);
      font-size: var(--body-strong-font-size, 16px);
      line-height: var(--body-strong-line-height, 140%);
      font-weight: var(--body-strong-font-weight, 600);
      position: relative;
      flex: 1;
    }
    .accordion-item-state-open .accordion-content {
      display: flex;
      flex-direction: row;
      gap: 0px;
      align-items: center;
      justify-content: center;
      align-self: stretch;
      flex-shrink: 0;
      position: relative;
    }
    .accordion-item-state-open .body {
      color: var(--var-sds-color-text-default-default, #1e1e1e);
      text-align: left;
      font-family: var(--body-base-font-family, "Inter-Regular", sans-serif);
      font-size: var(--body-base-font-size, 16px);
      line-height: var(--body-base-line-height, 140%);
      font-weight: var(--body-base-font-weight, 400);
      position: relative;
      flex: 1;
      display: flex;
      align-items: center;
      justify-content: flex-start;
    }
  </style>
  