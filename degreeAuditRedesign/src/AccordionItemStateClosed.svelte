<script>
  import { slide } from 'svelte/transition';
  import CompletedCourseItem from './CompletedCourseItem.svelte';
  export let title = '';
  export let state = '';
  export let className = '';
  export { className as class };
  export let completedCourses = [];

  // Sorting state
  let sortField = 'completedTerm';
  let sortDirection = 'desc';

  // Track expanded state separately from semester data
  let expandedTerms = new Set();

  function toggleSort(field) {
    if (sortField === field) {
      sortDirection = sortDirection === 'asc' ? 'desc' : 'asc';
    } else {
      sortField = field;
      sortDirection = 'asc';
    }
    // Re-group and sort courses while maintaining expanded states
    semesters = groupAndSortCourses(completedCourses);
  }

  function sortCourses(courses) {
    return [...courses].sort((a, b) => {
      let comparison = 0;
      switch (sortField) {
        case 'code':
          comparison = a.code.localeCompare(b.code);
          break;
        case 'name':
          comparison = a.name.localeCompare(b.name);
          break;
        case 'grade':
          comparison = b.grade.gpa - a.grade.gpa;
          break;
        case 'completedTerm':
          const [aTerm, aYear] = a.completedTerm.split(' ');
          const [bTerm, bYear] = b.completedTerm.split(' ');
          if (aYear !== bYear) {
            comparison = aYear - bYear;
          } else {
            const termOrder = { 'Spring': 1, 'Summer': 2, 'Fall': 3 };
            comparison = termOrder[aTerm] - termOrder[bTerm];
          }
          break;
      }
      return sortDirection === 'asc' ? comparison : -comparison;
    });
  }

  function groupAndSortCourses(courses) {
    const sortedCourses = sortCourses(courses);
    return sortedCourses.reduce((acc, course) => {
      if (!acc[course.completedTerm]) {
        acc[course.completedTerm] = {
          courses: [],
          // Use the tracked expanded state instead of always setting to false
          expanded: expandedTerms.has(course.completedTerm),
          totalHours: 0,
          gpa: 0
        };
      }
      acc[course.completedTerm].courses.push(course);
      acc[course.completedTerm].totalHours += course.creditHours;
      return acc;
    }, {});
  }

  function toggleSemester(term) {
    // Update the tracked expanded state
    if (expandedTerms.has(term)) {
      expandedTerms.delete(term);
    } else {
      expandedTerms.add(term);
    }
    // Update the semester data
    semesters[term].expanded = !semesters[term].expanded;
    semesters = semesters;
  }

  $: semesters = groupAndSortCourses(completedCourses);

  // Calculate GPA for each semester
  $: Object.keys(semesters).forEach(term => {
    const semesterCourses = semesters[term].courses;
    const totalGPAPoints = semesterCourses.reduce((sum, course) => sum + (course.grade.gpa * course.creditHours), 0);
    const totalHours = semesterCourses.reduce((sum, course) => sum + course.creditHours, 0);
    semesters[term].gpa = (totalGPAPoints / totalHours).toFixed(2);
    // Ensure expanded state is maintained
    semesters[term].expanded = expandedTerms.has(term);
  });
</script>

<div class={`accordion-item ${className}`}>
  <div class="accordion-header">
    <h2>{title}</h2>
  </div>
  <div class="sort-controls">
    <button 
      class:active={sortField === 'completedTerm'}
      class:asc={sortField === 'completedTerm' && sortDirection === 'asc'}
      on:click={() => toggleSort('completedTerm')}
    >
      Sort by Term {sortField === 'completedTerm' ? (sortDirection === 'asc' ? '↑' : '↓') : ''}
    </button>
    <button 
      class:active={sortField === 'code'}
      class:asc={sortField === 'code' && sortDirection === 'asc'}
      on:click={() => toggleSort('code')}
    >
      Sort by Code {sortField === 'code' ? (sortDirection === 'asc' ? '↑' : '↓') : ''}
    </button>
    <button 
      class:active={sortField === 'name'}
      class:asc={sortField === 'name' && sortDirection === 'asc'}
      on:click={() => toggleSort('name')}
    >
      Sort by Name {sortField === 'name' ? (sortDirection === 'asc' ? '↑' : '↓') : ''}
    </button>
    <button 
      class:active={sortField === 'grade'}
      class:asc={sortField === 'grade' && sortDirection === 'asc'}
      on:click={() => toggleSort('grade')}
    >
      Sort by Grade {sortField === 'grade' ? (sortDirection === 'asc' ? '↑' : '↓') : ''}
    </button>
  </div>
  <div class="accordion-content">
    {#each Object.entries(semesters) as [term, data]}
      <div class="semester-section">
        <div class="semester-header" on:click={() => toggleSemester(term)}>
          <div class="semester-info">
            <h3>{term}</h3>
            <div class="semester-stats">
              <span>{data.courses.length} Courses</span>
              <span class="separator">|</span>
              <span>{data.totalHours} Credit Hours</span>
              <span class="separator">|</span>
              <span>GPA: {data.gpa}</span>
            </div>
          </div>
          <div class="chevron" class:expanded={data.expanded}>
            ▼
          </div>
        </div>
        {#if data.expanded}
          <div class="semester-courses" transition:slide>
            {#each data.courses as course}
              <CompletedCourseItem {course} />
            {/each}
          </div>
        {/if}
      </div>
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

  .accordion-header h2 {
    margin: 0;
    font-size: 1.4rem;
    font-family: 'Inter', sans-serif;
    font-weight: 600;
    letter-spacing: 0.5px;
  }

  .accordion-content {
    padding: 24px;
    background: #1a1a1a;
  }

  .semester-section {
    margin-bottom: 16px;
  }

  .semester-header {
    background: #2a2a2a;
    padding: 16px;
    border-radius: 8px;
    display: flex;
    justify-content: space-between;
    align-items: center;
    cursor: pointer;
    border: 1px solid #333;
    transition: all 0.2s ease;
  }

  .semester-header:hover {
    background: #333;
    border-color: #444;
  }

  .semester-info h3 {
    margin: 0;
    color: #ffffff;
    font-size: 1.2rem;
    font-family: 'Inter', sans-serif;
    font-weight: 600;
  }

  .semester-stats {
    margin-top: 4px;
    color: #a0a0a0;
    font-size: 0.9rem;
    font-family: 'Inter', sans-serif;
    display: flex;
    align-items: center;
    gap: 8px;
  }

  .separator {
    color: #666;
  }

  .chevron {
    color: #666;
    transition: transform 0.3s ease;
  }

  .chevron.expanded {
    transform: rotate(180deg);
  }

  .semester-courses {
    padding: 16px 8px 0 8px;
  }

  /* Animation for expanding/collapsing */
  .semester-courses {
    animation: slideDown 0.3s ease-out;
  }

  @keyframes slideDown {
    from {
      opacity: 0;
      transform: translateY(-10px);
    }
    to {
      opacity: 1;
      transform: translateY(0);
    }
  }

  .sort-controls {
    display: flex;
    gap: 8px;
    padding: 16px 24px;
    background: #2d2d2d;
    border-bottom: 1px solid #333;
  }

  .sort-controls button {
    background: #1e1e1e;
    color: #a0a0a0;
    border: 1px solid #333;
    padding: 8px 16px;
    border-radius: 6px;
    cursor: pointer;
    font-family: 'Inter', sans-serif;
    font-size: 0.9rem;
    transition: all 0.2s ease;
  }

  .sort-controls button:hover {
    background: #2a2a2a;
    color: #ffffff;
    border-color: #444;
  }

  .sort-controls button.active {
    background: #333;
    color: #ffffff;
    border-color: #444;
  }
</style>