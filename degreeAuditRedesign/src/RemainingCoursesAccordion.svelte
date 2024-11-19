<script>
  import Chevron from "./lib/Chevron.svelte";
  import CourseItem from "./CourseItem.svelte";

  export let remainingCourses;
  export let searchTerm = "";

  let isCoreSectionOpen = true;
  let isElectiveSectionOpen = true;

  $: coreCourses = remainingCourses.filter(course => course.type === 'core');
  $: electiveCourses = remainingCourses.filter(course => course.type === 'elective');

  $: filteredCoreCourses = filterCourses(coreCourses, searchTerm);
  $: filteredElectiveCourses = filterCourses(electiveCourses, searchTerm);

  function createRegexFromWildcard(term) {
    // Escape special regex characters except *
    const escaped = term.replace(/[.+?^${}()|[\]\\]/g, '\\$&');
    // Replace * with .*
    const pattern = escaped.replace(/\*/g, '.*');
    // Create case-insensitive regex that matches the whole string
    return new RegExp(`^${pattern}$`, 'i');
  }

  function matchesWildcard(text, searchPattern) {
    // If the search pattern contains a wildcard
    if (searchPattern.includes('*')) {
      const regex = createRegexFromWildcard(searchPattern);
      return regex.test(text);
    }
    // If no wildcard, do a simple case-insensitive includes check
    return text.toLowerCase().includes(searchPattern.toLowerCase());
  }

  function filterCourses(courses, term) {
    if (!term) return courses;
    
    return courses.filter(course => {
      const searchTerms = term.toLowerCase().split(' ');
      
      return searchTerms.some(searchTerm => 
        // Check if any part of the course code matches
        course.code.split(/(?<=\D)(?=\d)|(?<=\d)(?=\D)/).some(part => 
          matchesWildcard(part, searchTerm)
        ) ||
        // Check if any word in the course name matches
        course.name.split(' ').some(word => 
          matchesWildcard(word, searchTerm)
        )
      );
    });
  }

  function toggleCoreSection() {
    isCoreSectionOpen = !isCoreSectionOpen;
  }

  function toggleElectiveSection() {
    isElectiveSectionOpen = !isElectiveSectionOpen;
  }

  // Update section states when search term changes
  $: {
    if (searchTerm) {
      // Auto-expand sections with matches
      if (filterCourses(coreCourses, searchTerm).length > 0) {
        isCoreSectionOpen = true;
      }
      if (filterCourses(electiveCourses, searchTerm).length > 0) {
        isElectiveSectionOpen = true;
      }
    }
  }
</script>

<div class="remaining-courses">
  <div class="accordion-section">
    <button 
      class="accordion-header" 
      on:click={toggleCoreSection}
      aria-expanded={isCoreSectionOpen}
      aria-controls="core-courses-content"
    >
      <div class="header-content">
        <h3>Core Requirements</h3>
        <div class="course-count">
          <span class="count-number">{coreCourses.length}</span>
          <span class="count-text">courses remaining</span>
        </div>
      </div>
      <div class="chevron" class:rotated={!isCoreSectionOpen}>
        <Chevron />
      </div>
    </button>
    {#if isCoreSectionOpen}
      <div id="core-courses-content" class="courses-list">
        {#each filteredCoreCourses as course (course.code)}
          <CourseItem 
            {course}
            highlighted={searchTerm && (
              course.code.split(/(?<=\D)(?=\d)|(?<=\d)(?=\D)/).some(part => 
                matchesWildcard(part, searchTerm)
              ) ||
              course.name.split(' ').some(word => 
                matchesWildcard(word, searchTerm)
              )
            )}
          />
        {/each}
      </div>
    {/if}
  </div>

  <div class="accordion-section">
    <button 
      class="accordion-header" 
      on:click={toggleElectiveSection}
      aria-expanded={isElectiveSectionOpen}
      aria-controls="elective-courses-content"
    >
      <div class="header-content">
        <h3>Elective Options</h3>
        <div class="course-count">
          <span class="count-number">{electiveCourses.length}</span>
          <span class="count-text">courses remaining</span>
        </div>
      </div>
      <div class="chevron" class:rotated={!isElectiveSectionOpen}>
        <Chevron />
      </div>
    </button>
    {#if isElectiveSectionOpen}
      <div id="elective-courses-content" class="courses-list">
        {#each filteredElectiveCourses as course (course.code)}
          <CourseItem 
            {course}
            highlighted={searchTerm && (
              course.code.split(/(?<=\D)(?=\d)|(?<=\d)(?=\D)/).some(part => 
                matchesWildcard(part, searchTerm)
              ) ||
              course.name.split(' ').some(word => 
                matchesWildcard(word, searchTerm)
              )
            )}
          />
        {/each}
      </div>
    {/if}
  </div>
</div>

<style>
  .remaining-courses {
    display: flex;
    flex-direction: column;
    gap: 12px;
  }

  .accordion-section {
    background: #252525;
    border-radius: 12px;
    border: 1px solid #333;
    overflow: hidden;
  }

  .accordion-header {
    width: 100%;
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 16px 20px;
    background: #2a2a2a;
    cursor: pointer;
    user-select: none;
    transition: background-color 0.2s ease;
    border: none;
    text-align: left;
  }

  .accordion-header:hover {
    background: #303030;
  }

  .header-content {
    display: flex;
    align-items: center;
    gap: 16px;
  }

  h3 {
    color: #ffffff;
    font-size: 18px;
    font-weight: 600;
    margin: 0;
  }

  .course-count {
    display: flex;
    align-items: center;
    gap: 6px;
    padding: 4px 12px;
    background: #1a1a1a;
    border-radius: 16px;
    border: 1px solid #333;
  }

  .count-number {
    color: #4CAF50;
    font-size: 16px;
    font-weight: 600;
  }

  .count-text {
    color: #888;
    font-size: 14px;
  }

  .chevron {
    transition: transform 0.3s ease;
  }

  .chevron.rotated {
    transform: rotate(180deg);
  }

  .courses-list {
    display: flex;
    flex-direction: column;
    padding: 8px;
  }

  .courses-list :global(.course-item) {
    margin-bottom: 2px;
  }

  .courses-list :global(.course-item:last-child) {
    margin-bottom: 0;
  }
</style> 