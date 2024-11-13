<script>
  import AccordionItemStateClosed from "./lib/AccordionItemStateClosed.svelte";
  import AccordionItemStateOpen from "./lib/AccoridonItemStateOpen.svelte";
  let className = "";
  export { className as class };
</script>
<main>
  <div class="{'desktop-1 ' + className}">
    <div class="course-list-frame">
      <div class="course-list-bounds"></div>
      <AccordionItemStateClosed
        title="Courses Completed"
        state="closed"
        class="completed-accordion-instance"
      ></AccordionItemStateClosed>
      <AccordionItemStateOpen
        content="Here will be the list of remaining classes. It will show them in order of needing to be completed. There will be dropdowns for each that show the options for the class. It will also show if you do or do not have the requirements for each course. This will be shown with a either green box that says enroll here or a red box that says prerequisites not met. When clicked it will show you the classes that are needed."
        title="Courses Remaining"
        class="remaining-accordion-instance"
      ></AccordionItemStateOpen>
    </div>
    <div class="degree-progress-text-frame">
      <div class="progress-text-bounds"></div>
      <div
        class="total-hours-100-hours-completed-60-hours-left-40-core-hours-left-30-elective-hours-left-10-gpa-3-0"
      >
        Total Hours: 100
        <br />
        Hours Completed: 60
        <br />
        Hours Left: 40
        <br />
        <br />
        Core Hours Left: 30
        <br />
        Elective Hours Left: 10
        <br />
        <br />
        GPA: 3.0
      </div>
    </div>
    <div class="degree-progress-chart-frame">
      <div class="progress-chat-bounds"></div>
      <div class="total-degree-progress">Total Degree Progress</div>
      <div class="pie-chart"></div>
    </div>
    <div class="line-1"></div>
    <div
      class="grant-guernsey-s-degree-audit-bachelor-of-science-computer-science"
    >
      Grant Guernsey’s Degree Audit - Bachelor of Science, Computer Science
    </div>
  </div>
  
</main>

<style>
  .desktop-1,
  .desktop-1 * {
    box-sizing: border-box;
  }
  .desktop-1 {
    background: #c3c9c3;
    height: 1024px;
    position: relative;
    box-shadow: 0px 4px 4px 0px rgba(0, 0, 0, 0.25);
    overflow: hidden;
  }
  .desktop-1 .course-list-frame {
    border-style: solid;
    border-color: transparent;
    border-width: 1px;
    width: 785px;
    height: 879px;
    position: absolute;
    left: 597px;
    top: 101px;
    box-shadow: 0px 4px 4px 0px rgba(0, 0, 0, 0.25);
  }
  .desktop-1 .course-list-bounds {
    background: #d9d9d9;
    border-radius: 25px;
    width: 785px;
    height: 879px;
    position: absolute;
    left: 0px;
    top: 0px;
    box-shadow: 0px 4px 4px 0px rgba(0, 0, 0, 0.25);
  }
  .desktop-1 :global(.completed-accordion-instance) {
    border-color: var(
      --var-sds-color-border-default-tertiary,
      #383838
    ) !important;
    width: 640px !important;
    position: absolute !important;
    left: 40px !important;
    top: 259px !important;
  }
  .desktop-1 :global(.remaining-accordion-instance) {
    border-color: var(
      --var-sds-color-border-default-tertiary,
      #383838
    ) !important;
    width: 640px !important;
    position: absolute !important;
    left: 40px !important;
    top: 31px !important;
  }
  .desktop-1 .degree-progress-text-frame {
    width: 460px;
    height: 383px;
    position: absolute;
    left: 25px;
    top: 597px;
  }
  .desktop-1 .progress-text-bounds {
    background: #d9d9d9;
    border-radius: 25px;
    width: 460px;
    height: 383px;
    position: absolute;
    left: 0px;
    top: 0px;
    box-shadow: 0px 4px 4px 0px rgba(0, 0, 0, 0.25);
  }
  .desktop-1
    .total-hours-100-hours-completed-60-hours-left-40-core-hours-left-30-elective-hours-left-10-gpa-3-0 {
    color: #000000;
    text-align: left;
    font-family: var(--subheading-font-family, "Inter-Regular", sans-serif);
    font-size: var(--subheading-font-size, 20px);
    line-height: var(--subheading-line-height, 120%);
    font-weight: var(--subheading-font-weight, 400);
    position: absolute;
    left: 30px;
    top: 38px;
    width: 380px;
    height: 185px;
    display: flex;
    align-items: center;
    justify-content: flex-start;
  }
  .desktop-1 .degree-progress-chart-frame {
    width: 460px;
    height: 470px;
    position: absolute;
    left: 25px;
    top: 101px;
  }
  .desktop-1 .progress-chat-bounds {
    background: #d9d9d9;
    border-radius: 25px;
    width: 460px;
    height: 470px;
    position: absolute;
    left: 0px;
    top: 0px;
    box-shadow: 0px 4px 4px 0px rgba(0, 0, 0, 0.25);
  }
  .desktop-1 .total-degree-progress {
    color: #000000;
    text-align: center;
    font-family: "Inter-SemiBold", sans-serif;
    font-size: 24px;
    font-weight: 600;
    position: absolute;
    left: 25px;
    top: 5px;
    width: 410px;
    height: 130px;
  }
  .desktop-1 .pie-chart {
    width: 400px;
    height: 400px;
    position: absolute;
    left: 30px;
    top: 35px;
  }
  .desktop-1 .line-1 {
    margin-top: -1px;
    border-style: solid;
    border-color: #000000;
    border-width: 1px 0 0 0;
    width: 1440.03px;
    height: 0px;
    position: absolute;
    left: 0px;
    top: 77px;
    transform-origin: 0 0;
    transform: rotate(0.386deg) scale(1, 1);
  }
  .desktop-1
    .grant-guernsey-s-degree-audit-bachelor-of-science-computer-science {
    color: #000000;
    text-align: center;
    font-family: "Inter-SemiBold", sans-serif;
    font-size: 32px;
    font-weight: 600;
    position: absolute;
    left: 65px;
    top: 20px;
    width: 1310px;
    height: 100px;
  }
</style>
