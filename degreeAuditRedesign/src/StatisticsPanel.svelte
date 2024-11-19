<script>
  export let completedCourses;
  export let remainingCourses;

  // Basic statistics
  $: totalCreditsCompleted = completedCourses.reduce((sum, course) => sum + course.creditHours, 0);
  $: totalCreditsRemaining = remainingCourses.reduce((sum, course) => sum + course.creditHours, 0);
  $: totalCredits = totalCreditsCompleted + totalCreditsRemaining;
  $: completionPercentage = Math.round((totalCreditsCompleted / totalCredits) * 100);

  // GPA calculations
  $: totalGPAPoints = completedCourses.reduce((sum, course) => sum + (course.grade.gpa * course.creditHours), 0);
  $: cumulativeGPA = Number(totalGPAPoints / totalCreditsCompleted).toFixed(2);
  
  // Grade distribution
  $: gradeDistribution = completedCourses.reduce((acc, course) => {
    const grade = course.grade.letter[0]; // Get first letter (A, B, C, etc.)
    acc[grade] = (acc[grade] || 0) + 1;
    return acc;
  }, {});

  // Core vs Elective progress
  $: coreRemaining = remainingCourses.filter(course => course.type === 'core').length;
  $: electivesRemaining = remainingCourses.filter(course => course.type === 'elective').length;
  
  // Term analysis
  $: currentTerm = "Spring 2024"; // You might want to make this dynamic
  $: coursesThisTerm = completedCourses.filter(course => course.completedTerm === currentTerm);
  $: gpaThisTerm = coursesThisTerm.length ? 
    (coursesThisTerm.reduce((sum, course) => sum + course.grade.gpa, 0) / coursesThisTerm.length).toFixed(2) : 
    "N/A";

  // Calculate academic standing
  $: academicStanding = Number(cumulativeGPA) >= 3.5 ? "Dean's List" :
                       Number(cumulativeGPA) >= 3.0 ? "Good Standing" :
                       Number(cumulativeGPA) >= 2.0 ? "Academic Warning" : "Academic Probation";

  // Estimated graduation
  $: remainingTerms = Math.ceil(totalCreditsRemaining / 15); // Assuming 15 credits per term is typical
</script>

<div class="statistics-container">

  <div class="stat-section academic-status">
    <h2>
      <span class="material-icons-round">analytics</span>
      Academic Performance
    </h2>
    <div class="performance-grid">
      <div class="performance-card">
        <div class="card-header">Cumulative GPA</div>
        <div class="gpa-display">
          <span class="large-stat">{cumulativeGPA}</span>
          <div class="standing-badge {academicStanding.toLowerCase().replace(/\s+/g, '-')}">
            {academicStanding}
          </div>
        </div>
      </div>
      <div class="performance-card">
        <div class="card-header">Current Term GPA</div>
        <div class="gpa-display">
          <span class="large-stat">{gpaThisTerm}</span>
          <span class="term-label">{currentTerm}</span>
        </div>
      </div>
    </div>
  </div>

  <div class="stat-section main-progress">
    <h2>
      <span class="material-icons-round">school</span>
      Degree Progress Overview
    </h2>
    <div class="progress-bar">
      <div class="progress" style="width: {completionPercentage}%"></div>
      <span class="progress-text">{completionPercentage}% Complete</span>
    </div>
    <div class="progress-details">
      <div class="stat-card">
        <span class="stat-value completed">{totalCreditsCompleted}</span>
        <span class="stat-label">Credits Completed</span>
      </div>
      <div class="stat-card">
        <span class="stat-value remaining">{totalCreditsRemaining}</span>
        <span class="stat-label">Credits Remaining</span>
      </div>
      <div class="stat-card">
        <span class="stat-value">{totalCredits}</span>
        <span class="stat-label">Total Required</span>
      </div>
    </div>
  </div>
  
  <div class="stat-section requirements">
    <h2>
      <span class="material-icons-round">checklist</span>
      Remaining Requirements
    </h2>
    <div class="requirements-grid">
      <div class="req-card">
        <span class="material-icons-round req-icon">book</span>
        <span class="req-count">{coreRemaining}</span>
        <span class="req-label">Core Courses</span>
      </div>
      <div class="req-card">
        <span class="material-icons-round req-icon">psychology</span>
        <span class="req-count">{electivesRemaining}</span>
        <span class="req-label">Electives</span>
      </div>
      <div class="req-card">
        <span class="material-icons-round req-icon">event</span>
        <span class="req-count">{remainingTerms}</span>
        <span class="req-label">Terms Left*</span>
      </div>
    </div>
    <div class="footnote">* Estimated based on 15 credits per term</div>
  </div>

  <div class="stat-section grade-distribution">
    <h2>
      <span class="material-icons-round">bar_chart</span>
      Grade Distribution
    </h2>
    <div class="grade-bars">
      {#each ['A', 'B', 'C', 'D', 'F'] as grade}
        <div class="grade-bar">
          <div class="bar-label">{grade}</div>
          <div class="bar-container">
            <div 
              class="bar"
              style="height: {((gradeDistribution[grade] || 0) / completedCourses.length * 100)}%"
            ></div>
          </div>
          <div class="bar-count">{gradeDistribution[grade] || 0}</div>
        </div>
      {/each}
    </div>
  </div>
</div>

<style>
  .statistics-container {
    display: flex;
    flex-direction: column;
    gap: 16px;
  }

  .stat-section {
    background: #252525;
    border-radius: 16px;
    padding: 20px;
    border: 1px solid #333;
  }

  h2 {
    display: flex;
    align-items: center;
    gap: 8px;
    font-size: 18px;
    font-weight: 600;
    margin: 0 0 16px 0;
    color: #ffffff;
  }

  .material-icons-round {
    font-size: 20px;
    color: #4CAF50;
  }

  /* Progress Bar Styles */
  .progress-bar {
    background: #1a1a1a;
    height: 24px;
    border-radius: 12px;
    position: relative;
    overflow: hidden;
    border: 1px solid #333;
    margin-bottom: 16px;
  }

  .progress {
    background: linear-gradient(90deg, #4CAF50, #8BC34A);
    height: 100%;
    transition: width 0.3s ease;
  }

  .progress-text {
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    color: #ffffff;
    font-size: 14px;
    font-weight: 600;
    text-shadow: 0 0 2px rgba(0, 0, 0, 0.5);
  }

  /* Progress Details */
  .progress-details {
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    gap: 12px;
  }

  .stat-card {
    background: #1a1a1a;
    padding: 12px;
    border-radius: 8px;
    text-align: center;
    border: 1px solid #333;
  }

  .stat-value {
    display: block;
    font-size: 24px;
    font-weight: 600;
    margin-bottom: 4px;
  }

  .stat-value.completed { color: #4CAF50; }
  .stat-value.remaining { color: #FFC107; }

  .stat-label {
    font-size: 12px;
    color: #888;
  }

  /* Academic Performance */
  .performance-grid {
    display: grid;
    grid-template-columns: repeat(2, 1fr);
    gap: 12px;
  }

  .performance-card {
    background: #1a1a1a;
    padding: 16px;
    border-radius: 8px;
    border: 1px solid #333;
  }

  .card-header {
    font-size: 14px;
    color: #888;
    margin-bottom: 8px;
  }

  .gpa-display {
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 8px;
  }

  .large-stat {
    font-size: 32px;
    font-weight: 600;
    color: #2196F3;
  }

  .standing-badge {
    padding: 4px 12px;
    border-radius: 12px;
    font-size: 12px;
    font-weight: 500;
  }

  .standing-badge.deans-list { background: #4CAF50; }
  .standing-badge.good-standing { background: #2196F3; }
  .standing-badge.academic-warning { background: #FFC107; }
  .standing-badge.academic-probation { background: #F44336; }

  .term-label {
    font-size: 14px;
    color: #888;
  }

  /* Requirements Section */
  .requirements-grid {
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    gap: 12px;
  }

  .req-card {
    background: #1a1a1a;
    padding: 16px;
    border-radius: 8px;
    border: 1px solid #333;
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 8px;
  }

  .req-icon {
    font-size: 24px;
    color: #2196F3;
  }

  .req-count {
    font-size: 24px;
    font-weight: 600;
    color: #ffffff;
  }

  .req-label {
    font-size: 12px;
    color: #888;
    text-align: center;
  }

  .footnote {
    font-size: 12px;
    color: #666;
    margin-top: 12px;
    text-align: center;
  }

  /* Grade Distribution */
  .grade-bars {
    display: flex;
    justify-content: space-between;
    align-items: flex-end;
    height: 120px;
    padding-top: 20px;
  }

  .grade-bar {
    display: flex;
    flex-direction: column;
    align-items: center;
    width: 40px;
  }

  .bar-label {
    color: #888;
    margin-bottom: 8px;
  }

  .bar-container {
    width: 24px;
    height: 80px;
    background: #1a1a1a;
    border-radius: 4px;
    overflow: hidden;
    position: relative;
  }

  .bar {
    width: 100%;
    background: #2196F3;
    position: absolute;
    bottom: 0;
    left: 0;
    transition: height 0.3s ease;
  }

  .bar-count {
    color: #888;
    margin-top: 8px;
    font-size: 12px;
  }
</style> 