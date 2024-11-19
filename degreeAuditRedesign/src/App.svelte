<script>
  import AccordionItemStateClosed from "./AccordionItemStateClosed.svelte";
  import RemainingCoursesAccordion from "./RemainingCoursesAccordion.svelte";
  import StatisticsPanel from './StatisticsPanel.svelte';
  import DegreeProgressChart from './DegreeProgressChart.svelte';
  import CourseSearch from './CourseSearch.svelte';
  import CompletedCoursesAccordion from './CompletedCoursesAccordion.svelte';
  
  let className = "";
  export { className as class };

  // Add search state
  let remainingSearchTerm = "";
  let completedSearchTerm = "";

  // Search handlers
  function handleRemainingSearch(term) {
    remainingSearchTerm = term;
  }

  function handleCompletedSearch(term) {
    completedSearchTerm = term;
  }

  // Initialize completedCourses with the data
  let completedCourses = [
    // Fall 2022
    {
      code: "CS1400",
      name: "Fundamentals of Programming",
      creditHours: 4,
      grade: { letter: "A", percentage: 95, gpa: 4.0 },
      completedTerm: "Fall 2022"
    },
    {
      code: "MATH2301",
      name: "Calculus I",
      creditHours: 4,
      grade: { letter: "B", percentage: 85, gpa: 3.0 },
      completedTerm: "Fall 2022"
    },
    {
      code: "ENG1500",
      name: "Writing and Rhetoric I",
      creditHours: 3,
      grade: { letter: "A-", percentage: 91, gpa: 3.7 },
      completedTerm: "Fall 2022"
    },
    // Spring 2023
    {
      code: "CS2400",
      name: "Introduction to Computer Science I",
      creditHours: 4,
      grade: { letter: "A-", percentage: 92, gpa: 3.7 },
      completedTerm: "Spring 2023"
    },
    {
      code: "MATH2302",
      name: "Calculus II",
      creditHours: 4,
      grade: { letter: "B+", percentage: 87, gpa: 3.3 },
      completedTerm: "Spring 2023"
    },
    {
      code: "PHYS2200",
      name: "Physics for Scientists & Engineers I",
      creditHours: 4,
      grade: { letter: "B", percentage: 85, gpa: 3.0 },
      completedTerm: "Spring 2023"
    },
    // Fall 2023
    {
      code: "CS2401",
      name: "Introduction to Computer Science II",
      creditHours: 4,
      grade: { letter: "B+", percentage: 88, gpa: 3.3 },
      completedTerm: "Fall 2023"
    },
    {
      code: "MATH3300",
      name: "Linear Algebra",
      creditHours: 3,
      grade: { letter: "A", percentage: 94, gpa: 4.0 },
      completedTerm: "Fall 2023"
    },
    {
      code: "PHYS2201",
      name: "Physics for Scientists & Engineers II",
      creditHours: 4,
      grade: { letter: "B", percentage: 86, gpa: 3.0 },
      completedTerm: "Fall 2023"
    },
    // Spring 2024
    {
      code: "CS3000",
      name: "Data Structures",
      creditHours: 3,
      grade: { letter: "A-", percentage: 91, gpa: 3.7 },
      completedTerm: "Spring 2024"
    },
    {
      code: "CS2450",
      name: "Software Engineering",
      creditHours: 3,
      grade: { letter: "A", percentage: 96, gpa: 4.0 },
      completedTerm: "Spring 2024"
    },
    {
      code: "MATH3302",
      name: "Discrete Mathematics",
      creditHours: 3,
      grade: { letter: "B+", percentage: 88, gpa: 3.3 },
      completedTerm: "Spring 2024"
    }
  ];

  // Updated remaining courses array
  let remainingCourses = [
    // Core Courses
    {
      code: "CS3100",
      name: "Operating Systems",
      creditHours: 3,
      type: "core",
      prerequisites: [
        {
          code: "CS3000",
          name: "Data Structures",
          completed: true
        }
      ]
    },
    {
      code: "CS3200",
      name: "Programming Languages",
      creditHours: 3,
      type: "core",
      prerequisites: [
        {
          code: "CS3000",
          name: "Data Structures",
          completed: true
        }
      ]
    },
    {
      code: "CS3500",
      name: "Database Systems",
      creditHours: 3,
      type: "core",
      prerequisites: [
        {
          code: "CS3000",
          name: "Data Structures",
          completed: true
        }
      ]
    },
    {
      code: "CS4100",
      name: "Computer Architecture",
      creditHours: 3,
      type: "core",
      prerequisites: [
        {
          code: "CS3100",
          name: "Operating Systems",
          completed: false
        }
      ]
    },
    // Elective Courses
    {
      code: "CS4200",
      name: "Artificial Intelligence",
      creditHours: 3,
      type: "elective",
      prerequisites: [
        {
          code: "CS3000",
          name: "Data Structures",
          completed: true
        },
        {
          code: "MATH3302",
          name: "Discrete Mathematics",
          completed: true
        }
      ]
    },
    {
      code: "CS4300",
      name: "Computer Graphics",
      creditHours: 3,
      type: "elective",
      prerequisites: [
        {
          code: "CS3000",
          name: "Data Structures",
          completed: true
        },
        {
          code: "MATH3300",
          name: "Linear Algebra",
          completed: true
        }
      ]
    },
    {
      code: "CS4400",
      name: "Computer Networks",
      creditHours: 3,
      type: "core",
      prerequisites: [
        {
          code: "CS3100",
          name: "Operating Systems",
          completed: false
        }
      ]
    },
    {
      code: "CS4500",
      name: "Machine Learning",
      creditHours: 3,
      type: "elective",
      prerequisites: [
        {
          code: "CS4200",
          name: "Artificial Intelligence",
          completed: false
        }
      ]
    },
    {
      code: "CS4600",
      name: "Cloud Computing",
      creditHours: 3,
      type: "elective",
      prerequisites: [
        {
          code: "CS3100",
          name: "Operating Systems",
          completed: false
        },
        {
          code: "CS4400",
          name: "Computer Networks",
          completed: false
        }
      ]
    }
  ];
</script>

<main>
  <div class={`desktop-1 ${className}`}>
    <div class="header">
      <div class="audit-title">
        Grant Guernsey's Degree Audit - Bachelor of Science, Computer Science
      </div>
      <div class="line-1"></div>
    </div>

    <div class="content-wrapper">
      <div class="left-column">
        <div class="degree-progress-chart-frame">
          <div class="total-degree-progress">Total Degree Progress</div>
          <DegreeProgressChart 
            {completedCourses} 
            {remainingCourses}
          />
        </div>

        <div class="degree-progress-text-frame">
          <StatisticsPanel 
            {completedCourses} 
            {remainingCourses}
          />
        </div>
      </div>

      <div class="right-column">
        <div class="course-list-frame">
          <div class="course-list-bounds">
            <div class="section-header">
              <div class="section-title">
                <span class="material-icons-round">assignment_late</span>
                Courses Remaining
              </div>
              <div class="total-count">
                <span class="count-number">{remainingCourses.length}</span>
                <span class="count-text">total courses</span>
              </div>
            </div>
            <CourseSearch 
              placeholder="Search remaining courses..." 
              onSearch={handleRemainingSearch}
            />
            <RemainingCoursesAccordion 
              {remainingCourses}
              searchTerm={remainingSearchTerm}
            />
            
            <div class="section-divider"></div>
            
            <div class="section-header">
              <div class="section-title">
                <span class="material-icons-round">task_alt</span>
                Courses Completed
              </div>
              <div class="total-count completed">
                <span class="count-number">{completedCourses.length}</span>
                <span class="count-text">total courses</span>
              </div>
            </div>
            <CourseSearch 
              placeholder="Search completed courses..." 
              onSearch={handleCompletedSearch}
            />
            <CompletedCoursesAccordion
              {completedCourses}
              searchTerm={completedSearchTerm}
            />
          </div>
        </div>
      </div>
    </div>
  </div>
</main>

<style>
  main {
    width: 100%;
    min-height: 100vh;
    background: #121212;
  }

  .desktop-1 {
    max-width: 1920px;
    min-height: 1080px;
    margin: 0 auto;
    padding: 20px 40px;
    position: relative;
    box-shadow: 0px 4px 4px 0px rgba(0, 0, 0, 0.25);
  }

  .header {
    width: 100%;
    margin-bottom: 40px;
  }

  .audit-title {
    color: #ffffff;
    text-align: center;
    font-family: 'Inter', sans-serif;
    font-size: 32px;
    font-weight: 600;
    margin-bottom: 20px;
  }

  .line-1 {
    width: 100%;
    height: 1px;
    background-color: #333;
  }

  .content-wrapper {
    display: flex;
    gap: 64px;
    padding: 20px 40px;
    max-width: 1800px;
    margin: 0 auto;
  }

  .left-column {
    flex: 0 0 420px;
    display: flex;
    flex-direction: column;
    gap: 40px;
  }

  .right-column {
    flex: 1;
    min-width: 0;
    max-width: calc(100% - 484px); /* 420px left column + 64px gap */
    display: flex;
    flex-direction: column;
  }

  .degree-progress-chart-frame {
    width: 100%;
    background: #1a1a1a;
    border-radius: 25px;
    padding: 24px;
    border: 1px solid #333;
    box-shadow: 0px 4px 4px 0px rgba(0, 0, 0, 0.25);
  }

  .degree-progress-text-frame {
    width: 100%;
    background: #1a1a1a;
    border-radius: 25px;
    padding: 24px;
    border: 1px solid #333;
    box-shadow: 0px 4px 4px 0px rgba(0, 0, 0, 0.25);
  }

  .total-degree-progress {
    color: #ffffff;
    text-align: center;
    font-family: 'Inter', sans-serif;
    font-size: 24px;
    font-weight: 600;
    margin-bottom: 20px;
    display: flex;
    align-items: center;
    justify-content: center;
    gap: 12px;
  }

  .total-degree-progress::before {
    content: "donut_large";
    font-family: 'Material Icons Round';
    font-size: 28px;
  }

  .pie-chart {
    width: 350px;
    height: 350px;
    margin: 0 auto;
  }

  .progress-stats {
    color: #ffffff;
    font-family: 'Inter', sans-serif;
    font-size: 20px;
    line-height: 1.5;
    padding: 20px;
  }

  .course-list-frame {
    width: 100%;
    box-sizing: border-box;
  }

  .course-list-bounds {
    background: #1a1a1a;
    border-radius: 25px;
    padding: 32px;
    border: 1px solid #333;
    box-shadow: 0px 4px 4px 0px rgba(0, 0, 0, 0.25);
    margin-left: 12px;
    width: 100%;
    box-sizing: border-box;
  }

  :global(.remaining-accordion-instance),
  :global(.completed-accordion-instance) {
    width: 100% !important;
    position: relative !important;
    left: 0 !important;
    margin-bottom: 24px !important;
  }

  :global(.remaining-accordion-instance) {
    margin-bottom: 32px !important;
  }

  .section-title {
    color: #ffffff;
    font-size: 24px;
    font-weight: 600;
    display: flex;
    align-items: center;
    gap: 12px;
  }

  .section-title .material-icons-round {
    font-size: 28px;
  }

  .section-divider {
    height: 1px;
    background: #333;
    margin: 32px 0;
  }

  .section-header {
    display: flex;
    align-items: center;
    justify-content: space-between;
    margin-bottom: 20px;
  }

  .total-count {
    display: flex;
    align-items: center;
    gap: 8px;
    padding: 6px 16px;
    background: #252525;
    border-radius: 20px;
    border: 1px solid #333;
  }

  .total-count.completed .count-number {
    color: #2196F3; /* Different color for completed courses count */
  }

  .count-number {
    color: #4CAF50;
    font-size: 20px;
    font-weight: 600;
  }

  .count-text {
    color: #888;
    font-size: 16px;
  }

  /* Add styles for search container */
  :global(.search-container) {
    width: 100%;
    box-sizing: border-box;
    padding: 0;
    margin-bottom: 20px;
  }

  :global(.search-input) {
    box-sizing: border-box;
    width: 100%;
  }
</style>
