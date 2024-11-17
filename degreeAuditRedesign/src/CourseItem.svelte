<script>
  import { slide } from 'svelte/transition';
  import { onMount } from 'svelte';

  export let course;
  export let isExpanded = false;
  export let highlighted = false;

  let isEnrolled = false;

  function toggleExpand() {
    isExpanded = !isExpanded;
  }

  function canEnroll() {
    return course.prerequisites ? course.prerequisites.every(prereq => prereq.completed) : false;
  }

  function enroll() {
    isEnrolled = true;
    alert('You have successfully enrolled in the course!');
  }

  function handleKeyDown(event) {
    if (event.key === 'Enter' || event.key === ' ') {
      event.preventDefault();
      toggleExpand();
    }
  }

  $: isCompletedCourse = course.grade !== undefined;
</script>

<div class="course-item" class:highlighted class:expanded={isExpanded}>
  <div 
    class="course-header" 
    on:click={toggleExpand}
    on:keydown={handleKeyDown}
    role="button"
    tabindex="0"
    aria-expanded={isExpanded}
  >
    <div class="course-info">
      <div class="course-title">
        <span class="course-code">{course.code}</span>
        <span class="course-name">{course.name}</span>
      </div>
      <div class="course-details">
        <span class="credit-hours">{course.creditHours} Credit Hours</span>
        {#if course.type}
          <span class="separator">•</span>
          <span class="course-type">{course.type}</span>
        {/if}
        {#if isCompletedCourse}
          <span class="separator">•</span>
          <span class="grade">Grade: {course.grade.letter}</span>
        {/if}
      </div>
    </div>
    {#if !isCompletedCourse}
      <div class="action-container">
        {#if canEnroll()}
          <button class="action-button enroll" on:click={enroll} disabled={isEnrolled}>
            <span class="material-icons-round">check_circle</span>
            {isEnrolled ? 'Enrolled' : 'Enroll Here'}
          </button>
        {:else}
          <button class="action-button prerequisites" class:expanded={isExpanded}>
            <span class="material-icons-round">warning</span>
            Prerequisites Not Met
          </button>
        {/if}
      </div>
    {:else}
      <div class="grade-display">
        <span class="material-icons-round grade-icon">
          {course.grade.gpa >= 3.0 ? 'grade' : 'assignment'}
        </span>
        <div class="grade-details">
          <span class="grade-value">{course.grade.letter}</span>
          <span class="gpa-value">({course.grade.gpa.toFixed(1)} GPA)</span>
        </div>
      </div>
    {/if}
  </div>

  {#if isExpanded && !isCompletedCourse && !canEnroll()}
    <div 
      class="prerequisites-list" 
      transition:slide
      role="region"
      aria-label="Course prerequisites"
    >
      <h4>Required Prerequisites:</h4>
      <ul>
        {#each course.prerequisites as prereq}
          <li class:completed={prereq.completed}>
            <span class="prereq-code">{prereq.code}</span>
            <span class="prereq-name">{prereq.name}</span>
            <span class="prereq-status">
              <span class="material-icons-round">
                {prereq.completed ? 'check_circle' : 'cancel'}
              </span>
              {prereq.completed ? 'Completed' : 'Not Completed'}
            </span>
          </li>
        {/each}
      </ul>
    </div>
  {/if}
</div>

<style>
  .course-item {
    background: #1e1e1e;
    border-radius: 12px;
    padding: 16px 20px;
    box-shadow: 0 4px 6px rgba(0, 0, 0, 0.2);
    transition: all 0.3s ease;
    border: 1px solid #333;
    margin: 2px 0;
    min-height: 76px;
    box-sizing: border-box;
    display: flex;
    flex-direction: column;
    justify-content: center;
    position: relative;
    z-index: 1;
  }

  .course-item.expanded {
    z-index: 2;
    height: auto;
  }

  .course-item:hover {
    transform: translateY(-2px);
    box-shadow: 0 6px 12px rgba(0, 0, 0, 0.3);
    border-color: #444;
  }

  .course-header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    gap: 12px;
    cursor: pointer;
  }

  .course-info {
    flex: 1;
    min-width: 0;
    margin-right: 4px;
  }

  .course-title {
    display: flex;
    align-items: center;
    gap: 10px;
    margin-bottom: 6px;
    min-width: 0;
  }

  .course-code {
    font-family: 'Inter', sans-serif;
    font-size: 1.1rem;
    font-weight: 600;
    color: #3498db;
    white-space: nowrap;
    flex-shrink: 0;
  }

  .course-name {
    font-family: 'Inter', sans-serif;
    font-size: 1.1rem;
    font-weight: 500;
    color: #ffffff;
    white-space: nowrap;
    overflow: hidden;
    text-overflow: ellipsis;
    min-width: 0;
  }

  .course-details {
    display: flex;
    align-items: center;
    gap: 8px;
    color: #a0a0a0;
    font-size: 0.9rem;
    font-family: 'Inter', sans-serif;
  }

  .separator {
    color: #666;
  }

  .course-type {
    text-transform: capitalize;
  }

  .action-container {
    flex-shrink: 0;
  }

  .action-button {
    display: flex;
    align-items: center;
    gap: 4px;
    padding: 6px 12px;
    border-radius: 4px;
    border: none;
    font-family: 'Inter', sans-serif;
    font-size: 0.8rem;
    font-weight: 500;
    text-transform: uppercase;
    letter-spacing: 0.3px;
    cursor: pointer;
    transition: all 0.2s ease;
    white-space: nowrap;
    min-width: fit-content;
  }

  .action-button.enroll {
    background: #2ecc71;
    color: #ffffff;
  }

  .action-button.enroll:hover {
    background: #27ae60;
    transform: translateY(-1px);
  }

  .action-button.prerequisites {
    background: #e74c3c;
    color: #ffffff;
  }

  .action-button.prerequisites:hover {
    background: #c0392b;
    transform: translateY(-1px);
  }

  .prerequisites-list {
    margin-top: 20px;
    padding: 20px;
    background: #2a2a2a;
    border-radius: 8px;
    border: 1px solid #333;
    position: relative;
    z-index: 2;
  }

  .prerequisites-list h4 {
    margin: 0 0 16px 0;
    color: #ffffff;
    font-size: 1rem;
    font-weight: 600;
    font-family: 'Inter', sans-serif;
  }

  .prerequisites-list ul {
    margin: 0;
    padding: 0;
    list-style: none;
  }

  .prerequisites-list li {
    display: flex;
    align-items: center;
    gap: 12px;
    padding: 12px;
    margin-bottom: 8px;
    background: #1e1e1e;
    border-radius: 6px;
    border: 1px solid #333;
  }

  .prereq-code {
    color: #3498db;
    font-weight: 600;
    white-space: nowrap;
  }

  .prereq-name {
    flex: 1;
    color: #e0e0e0;
  }

  .prereq-status {
    display: flex;
    align-items: center;
    gap: 4px;
    color: #e74c3c;
    font-size: 0.9rem;
  }

  .completed .prereq-status {
    color: #2ecc71;
  }

  /* Ensure consistent font family */
  .prerequisites-list li,
  .prereq-code,
  .prereq-name,
  .prereq-status {
    font-family: 'Inter', sans-serif;
    font-size: 0.95rem;
  }

  .highlighted {
    background: rgba(76, 175, 80, 0.1);
    border-color: #4CAF50;
  }

  .material-icons-round {
    font-size: 16px;
  }

  .grade-display {
    display: flex;
    align-items: center;
    gap: 8px;
    padding: 8px 16px;
    background: #252525;
    border-radius: 20px;
    border: 1px solid #333;
  }

  .grade-icon {
    color: #2196F3;
    font-size: 20px;
  }

  .grade-details {
    display: flex;
    align-items: center;
    gap: 6px;
  }

  .grade-value {
    color: #ffffff;
    font-weight: 600;
    font-size: 16px;
  }

  .gpa-value {
    color: #888;
    font-size: 14px;
  }

  .grade {
    color: #2196F3;
    font-weight: 500;
  }

  .course-name {
    position: relative;
  }

  .course-name:hover {
    overflow: visible;
    white-space: normal;
    z-index: 1;
    background: #1e1e1e;
    border-radius: 4px;
    box-shadow: 0 2px 8px rgba(0, 0, 0, 0.2);
    padding: 2px 6px;
    margin: -2px -6px;
  }
</style>