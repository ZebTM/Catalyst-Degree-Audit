<script>
  export let course;

  // Function to determine grade color
  function getGradeColor(grade) {
    const letter = grade.charAt(0);
    if (grade === 'A' || grade === 'A-') return '#2ecc71';
    if (letter === 'B') return '#3498db';
    if (letter === 'C') return '#f1c40f';
    if (letter === 'D') return '#e67e22';
    if (letter === 'F') return '#e74c3c';
    return '#ffffff';
  }
</script>

<div class="course-item">
  <div class="course-header">
    <div class="course-info">
      <div class="course-title">
        <span class="course-code">{course.code}</span>
        <span class="course-name">{course.name}</span>
      </div>
      <div class="course-details">
        <span class="credit-hours">{course.creditHours} Credit Hours</span>
        <span class="separator">•</span>
        <span class="term">{course.completedTerm}</span>
      </div>
    </div>
    <div class="grade-container">
      <div class="grade-box">
        <span class="grade-letter" style="color: {getGradeColor(course.grade.letter)}">
          {course.grade.letter}
        </span>
        <div class="grade-details">
          <span class="grade-percentage">{course.grade.percentage}%</span>
          <span class="separator">|</span>
          <span class="grade-gpa">GPA: {course.grade.gpa.toFixed(1)}</span>
        </div>
      </div>
    </div>
  </div>
</div>

<style>
  .course-item {
    background: #1e1e1e;
    border-radius: 12px;
    margin: 16px 0;
    padding: 20px;
    box-shadow: 0 4px 6px rgba(0, 0, 0, 0.2);
    transition: all 0.3s ease;
    border: 1px solid #333;
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
    gap: 24px;
  }

  .course-info {
    flex: 1;
    min-width: 0;
  }

  .course-title {
    display: flex;
    align-items: center;
    gap: 12px;
    margin-bottom: 8px;
  }

  .course-code {
    font-family: 'Inter', sans-serif;
    font-size: 1.1rem;
    font-weight: 600;
    color: #3498db;
    white-space: nowrap;
  }

  .course-name {
    font-family: 'Inter', sans-serif;
    font-size: 1.1rem;
    font-weight: 500;
    color: #ffffff;
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

  .term {
    font-style: italic;
  }

  .grade-container {
    flex-shrink: 0;
  }

  .grade-box {
    display: flex;
    align-items: center;
    gap: 12px;
    padding: 12px 20px;
    background: #2a2a2a;
    border-radius: 8px;
    border: 1px solid #444;
  }

  .grade-letter {
    font-size: 1.4rem;
    font-weight: 700;
    font-family: 'Inter', sans-serif;
    white-space: nowrap;
  }

  .grade-details {
    display: flex;
    align-items: center;
    gap: 8px;
    color: #ffffff;
    font-size: 0.9rem;
    font-family: 'Inter', sans-serif;
    white-space: nowrap;
  }

  .grade-percentage, .grade-gpa {
    color: #e0e0e0;
  }
</style>
 