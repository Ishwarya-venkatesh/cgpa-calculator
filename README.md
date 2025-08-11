# cgpa-calculator 
//html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Interactive CGPA Calculator</title>
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <link rel="stylesheet" href="style.css">
</head>
<body>
  <div class="container">
    <h1>🎓 Interactive CGPA Calculator</h1>

    <div class="controls">
      <button onclick="addCourse()">➕ Add Course</button>
      <button onclick="resetAll()">🔁 Reset</button>
      <button onclick="toggleTheme()">🌓 Toggle Theme</button>
    </div>

    <div id="courseList"></div>

    <div class="target">
      🎯 Target CGPA: <input type="number" step="0.01" id="targetCgpa" placeholder="e.g. 3.5">
    </div>

    <div class="cgpa-display">
      <div id="progressCircle">
        <span id="cgpaText">CGPA: 0.00</span>
      </div>
    </div>

  </div>

  <script src="script.js"></script>



  //css

  body {
  font-family: 'Segoe UI', sans-serif;
  margin: 0;
  padding: 0;
  background-color: var(--bg);
  color: var(--text);
  transition: all 0.3s ease;
}

:root {
  --bg: #f5f5f5;
  --text: #222;
  --accent: #0077ff;
  --card: #ffffff;
}

body.dark {
  --bg: #121212;
  --text: #ffffff;
  --accent: #66ccff;
  --card: #1e1e1e;
}

.container {
  max-width: 800px;
  margin: auto;
  padding: 2rem;
}

h1 {
  text-align: center;
  color: var(--accent);
}

.controls {
  display: flex;
  justify-content: center;
  gap: 1rem;
  margin-bottom: 1rem;
}

button {
  padding: 0.5rem 1rem;
  font-size: 1rem;
  background-color: var(--accent);
  color: white;
  border: none;
  border-radius: 6px;
  cursor: pointer;
}

button:hover {
  opacity: 0.8;
}

.course {
  display: flex;
  gap: 1rem;
  background-color: var(--card);
  padding: 1rem;
  margin-bottom: 0.5rem;
  border-radius: 8px;
  align-items: center;
}

.course input {
  padding: 0.5rem;
  border: 1px solid #ccc;
  border-radius: 6px;
  flex: 1;
}

.remove-btn {
  background-color: crimson;
}

.cgpa-display {
  margin-top: 2rem;
  display: flex;
  justify-content: center;
}

#progressCircle {
  width: 150px;
  height: 150px;
  background: conic-gradient(var(--accent) 0%, #ccc 0%);
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 1.2rem;
  color: var(--text);
  font-weight: bold;
}

.target {
  text-align: center;
  margin: 1rem 0;
}


//script

let courseList = document.getElementById('courseList');
let cgpaText = document.getElementById('cgpaText');
let progressCircle = document.getElementById('progressCircle');
let courseId = 0;

function addCourse(name = "", credit = "", grade = "") {
  const div = document.createElement('div');
  div.className = 'course';
  div.innerHTML = `
    <input type="text" placeholder="Course Name" value="${name}">
    <input type="number" placeholder="Credit" value="${credit}" min="0">
    <input type="number" placeholder="Grade Point (0-4)" value="${grade}" min="0" max="4" step="0.01">
    <button class="remove-btn" onclick="removeCourse(this)">❌</button>
  `;
  courseList.appendChild(div);
  div.querySelectorAll('input').forEach(input => {
    input.addEventListener('input', calculateCGPA);
  });
  calculateCGPA();
}

function removeCourse(btn) {
  btn.parentElement.remove();
  calculateCGPA();
}

function calculateCGPA() {
  const courses = document.querySelectorAll('.course');
  let totalCredits = 0;
  let totalPoints = 0;
  courses.forEach(course => {
    const credit = parseFloat(course.children[1].value);
    const grade = parseFloat(course.children[2].value);
    if (!isNaN(credit) && !isNaN(grade)) {
      totalCredits += credit;
      totalPoints += grade * credit;
    }
  });

  let cgpa = totalCredits ? (totalPoints / totalCredits) : 0;
  cgpa = cgpa.toFixed(2);
  cgpaText.textContent = `CGPA: ${cgpa}`;

  // Set progress circle
  let percent = Math.min((cgpa / 4) * 100, 100);
  progressCircle.style.background = `conic-gradient(var(--accent) ${percent}%, #ccc ${percent}%)`;

  // Compare with target
  const target = parseFloat(document.getElementById('targetCgpa').value);
  if (!isNaN(target)) {
    if (cgpa >= target) {
      cgpaText.style.color = 'limegreen';
    } else {
      cgpaText.style.color = 'orangered';
    }
  } else {
    cgpaText.style.color = 'var(--text)';
  }
}

function resetAll() {
  courseList.innerHTML = '';
  cgpaText.textContent = 'CGPA: 0.00';
  progressCircle.style.background = 'conic-gradient(var(--accent) 0%, #ccc 0%)';
}

function toggleTheme() {
  document.body.classList.toggle('dark');
}

</body>
</html>
