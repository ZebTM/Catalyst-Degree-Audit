<script>
  import Chevron from "./lib/Chevron.svelte";
  import CourseItem from "./CourseItem.svelte";

  export let completedCourses;
  export let searchTerm = "";

  let viewMode = "term"; // "term" or "year"
  let openSections = new Set(); // Initialize as regular Set, not reactive

  // Group courses by term
  $: coursesByTerm = completedCourses.reduce((acc, course) => {
    if (!acc[course.completedTerm]) {
      acc[course.completedTerm] = [];
    }
    acc[course.completedTerm].push(course);
    return acc;
  }, {});

  // Group courses by year
  $: coursesByYear = completedCourses.reduce((acc, course) => {
    const year = course.completedTerm.split(" ")[1];
    if (!acc[year]) {
      acc[year] = [];
    }
    acc[year].push(course);
    return acc;
  }, {});

  // Sort terms in reverse chronological order
  $: sortedTerms = Object.keys(coursesByTerm).sort((a, b) => {
    const [termA, yearA] = a.split(" ");
    const [termB, yearB] = b.split(" ");
    const yearDiff = Number(yearB) - Number(yearA);
    if (yearDiff !== 0) return yearDiff;
    const termOrder = { Spring: 0, Summer: 1, Fall: 2 };
    return termOrder[termB] - termOrder[termA];
  });

  // Sort years in reverse chronological order
  $: sortedYears = Object.keys(coursesByYear).sort((a, b) => Number(b) - Number(a));

  function toggleSection(section) {
    const newOpenSections = new Set(openSections);
    if (newOpenSections.has(section)) {
      newOpenSections.delete(section);
    } else {
      newOpenSections.add(section);
    }
    openSections = newOpenSections;
  }

  // Filter courses based on search term
  function filterCourses(courses) {
    if (!searchTerm) return courses;
    const term = searchTerm.toLowerCase();
    return courses.filter(course => 
      course.code.toLowerCase().includes(term) ||
      course.name.toLowerCase().includes(term)
    );
  }

  // Sort courses within a term by course code
  function sortCourses(courses) {
    return [...courses].sort((a, b) => a.code.localeCompare(b.code));
  }

  // Modified to only set default open section on initial load
  let initialized = false;
  $: {
    if (!initialized && ((viewMode === 'term' && sortedTerms.length > 0) || 
        (viewMode === 'year' && sortedYears.length > 0))) {
      openSections = new Set([viewMode === 'term' ? sortedTerms[0] : sortedYears[0]]);
      initialized = true;
    }
  }

  // Reset initialized state when view mode changes
  $: {
    if (viewMode) {
      initialized = false;
    }
  }

  // Calculate credit hours for each term
  $: creditsByTerm = Object.entries(coursesByTerm).reduce((acc, [term, courses]) => {
    acc[term] = courses.reduce((sum, course) => sum + course.creditHours, 0);
    return acc;
  }, {});

  // Calculate credit hours for each year
  $: creditsByYear = Object.entries(coursesByYear).reduce((acc, [year, courses]) => {
    acc[year] = courses.reduce((sum, course) => sum + course.creditHours, 0);
    return acc;
  }, {});

  // Update openSections when search term changes
  $: {
    if (searchTerm) {
      const newOpenSections = new Set(openSections);
      
      if (viewMode === 'term') {
        sortedTerms.forEach(term => {
          const hasMatch = filterCourses(coursesByTerm[term]).length > 0;
          if (hasMatch) {
            newOpenSections.add(term);
          }
        });
      } else {
        sortedYears.forEach(year => {
          const hasMatch = filterCourses(coursesByYear[year]).length > 0;
          if (hasMatch) {
            newOpenSections.add(year);
          }
        });
      }
      
      openSections = newOpenSections;
    }
  }

  function matchesWildcard(text, searchPattern) {
    // If the search pattern contains a wildcard
    if (searchPattern.includes('*')) {
      const escaped = searchPattern.replace(/[.+?^${}()|[\]\\]/g, '\\$&');
      const pattern = escaped.replace(/\*/g, '.*');
      const regex = new RegExp(`^${pattern}$`, 'i');
      return regex.test(text);
    }
    // If no wildcard, do a simple case-insensitive includes check
    return text.toLowerCase().includes(searchPattern.toLowerCase());
  }
</script>

<div class="view-switcher">
  <button 
    class="view-button" 
    class:active={viewMode === "term"}
    on:click={() => viewMode = "term"}
  >
    <span class="material-icons-round">calendar_month</span>
    View by Term
  </button>
  <button 
    class="view-button" 
    class:active={viewMode === "year"}
    on:click={() => viewMode = "year"}
  >
    <span class="material-icons-round">calendar_today</span>
    View by Year
  </button>
</div>

{#if viewMode === "term"}
  <div class="accordion-container">
    {#each sortedTerms as term (term)}
      <div class="accordion-section">
        <button 
          class="accordion-header" 
          on:click={() => toggleSection(term)}
          aria-expanded={openSections.has(term)}
        >
          <div class="header-content">
            <h3>{term}</h3>
            <div class="course-count">
              <span class="count-number">{coursesByTerm[term].length}</span>
              <span class="count-text">courses</span>
              <span class="separator">•</span>
              <span class="credit-number">{creditsByTerm[term]}</span>
              <span class="count-text">credits</span>
            </div>
          </div>
          <div class="chevron" class:rotated={!openSections.has(term)}>
            <Chevron />
          </div>
        </button>
        {#if openSections.has(term)}
          <div class="courses-list">
            {#each sortCourses(filterCourses(coursesByTerm[term])) as course (course.code)}
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
    {/each}
  </div>
{:else}
  <div class="accordion-container">
    {#each sortedYears as year (year)}
      <div class="accordion-section">
        <button 
          class="accordion-header" 
          on:click={() => toggleSection(year)}
          aria-expanded={openSections.has(year)}
        >
          <div class="header-content">
            <h3>{year}</h3>
            <div class="course-count">
              <span class="count-number">{coursesByYear[year].length}</span>
              <span class="count-text">courses</span>
              <span class="separator">•</span>
              <span class="credit-number">{creditsByYear[year]}</span>
              <span class="count-text">credits</span>
            </div>
          </div>
          <div class="chevron" class:rotated={!openSections.has(year)}>
            <Chevron />
          </div>
        </button>
        {#if openSections.has(year)}
          <div class="courses-list">
            {#each sortCourses(filterCourses(coursesByYear[year])) as course (course.code)}
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
    {/each}
  </div>
{/if}

<style>
  .view-switcher {
    display: flex;
    gap: 12px;
    margin-bottom: 20px;
  }

  .view-button {
    display: flex;
    align-items: center;
    gap: 8px;
    padding: 8px 16px;
    background: #252525;
    border: 1px solid #333;
    border-radius: 8px;
    color: #888;
    font-size: 14px;
    cursor: pointer;
    transition: all 0.2s ease;
  }

  .view-button:hover {
    background: #2a2a2a;
    color: #fff;
  }

  .view-button.active {
    background: #2a2a2a;
    color: #fff;
    border-color: #4CAF50;
  }

  .view-button .material-icons-round {
    font-size: 18px;
  }

  .accordion-container {
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

  .count-number, .credit-number {
    color: #2196F3;
    font-size: 16px;
    font-weight: 600;
  }

  .separator {
    color: #444;
    margin: 0 2px;
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