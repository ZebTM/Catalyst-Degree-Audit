<script>
  import CourseItem from './CourseItem.svelte';
  export let title = '';
  export let className = "";
  export { className as class };
  export let remainingCourses = [];

  $: totalRemainingCourses = remainingCourses.length;
  $: coreCoursesCount = remainingCourses.filter(course => course.type === 'core').length;
  $: electiveCoursesCount = remainingCourses.filter(course => course.type === 'elective').length;
</script>

<div class={`accordion-item ${className}`}>
  <div class="accordion-header">
    <div class="header-content">
      <h2>{title}</h2>
      <div class="course-stats">
        <div class="stat-item">
          <span class="stat-value">{totalRemainingCourses}</span>
          <span class="stat-label">Total Courses</span>
        </div>
        <div class="separator">|</div>
        <div class="stat-item">
          <span class="stat-value core">{coreCoursesCount}</span>
          <span class="stat-label">Core</span>
        </div>
        <div class="separator">|</div>
        <div class="stat-item">
          <span class="stat-value elective">{electiveCoursesCount}</span>
          <span class="stat-label">Electives</span>
        </div>
      </div>
    </div>
  </div>
  <div class="accordion-content">
    {#each remainingCourses as course}
      <CourseItem {course} />
    {/each}
  </div>
</div>

<style>
  .accordion-item {
    background: #1a1a1a;
    border-radius: 12px;
    overflow: hidden;
    margin-bottom: 16px;
    border: 1px solid #333;
  }

  .accordion-header {
    background: #2d2d2d;
    color: #ffffff;
    padding: 20px;
    border-bottom: 1px solid #333;
  }

  .header-content {
    display: flex;
    justify-content: space-between;
    align-items: center;
  }

  .accordion-header h2 {
    margin: 0;
    font-size: 1.4rem;
    font-family: 'Inter', sans-serif;
    font-weight: 600;
    letter-spacing: 0.5px;
  }

  .course-stats {
    display: flex;
    align-items: center;
    gap: 16px;
    padding: 8px 16px;
    background: #1e1e1e;
    border-radius: 8px;
    border: 1px solid #333;
  }

  .stat-item {
    display: flex;
    flex-direction: column;
    align-items: center;
    min-width: 80px;
  }

  .stat-value {
    font-size: 1.2rem;
    font-weight: 600;
    font-family: 'Inter', sans-serif;
    color: #ffffff;
  }

  .stat-value.core {
    color: #3498db;
  }

  .stat-value.elective {
    color: #9b59b6;
  }

  .stat-label {
    font-size: 0.8rem;
    color: #a0a0a0;
    text-transform: uppercase;
    letter-spacing: 0.5px;
    margin-top: 2px;
  }

  .separator {
    color: #666;
    font-size: 1.2rem;
  }

  .accordion-content {
    padding: 24px;
    background: #1a1a1a;
  }
</style> 