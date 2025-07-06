# studentandtecherdata
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Student & Teacher Manager</title>
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/css/bootstrap.min.css" rel="stylesheet"/>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/xlsx/0.18.5/xlsx.full.min.js"></script>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf-autotable/3.5.25/jspdf.plugin.autotable.min.js"></script>
  <style>
    body { background-color: #f8f9fa; }
    .card { box-shadow: 0 0 10px rgba(0,0,0,0.05); }
  </style>
</head>
<body>

<div class="container py-5">
  <h2 class="text-center mb-4">🎓 Student & 👨‍🏫 Teacher Management</h2>

  <div class="row g-4">
    <!-- Student Form -->
    <div class="col-md-6">
      <div class="card">
        <div class="card-header bg-primary text-white">➕ Add Student</div>
        <div class="card-body">
          <form id="studentForm">
            <div class="mb-3">
              <label class="form-label">Full Name</label>
              <input type="text" id="studentName" class="form-control" placeholder="Enter student name" required>
            </div>
            <button class="btn btn-primary w-100" type="submit">Add Student</button>
          </form>
        </div>
      </div>
    </div>

    <!-- Teacher Form -->
    <div class="col-md-6">
      <div class="card">
        <div class="card-header bg-success text-white">➕ Add Teacher</div>
        <div class="card-body">
          <form id="teacherForm">
            <div class="mb-3">
              <label class="form-label">Full Name</label>
              <input type="text" id="teacherName" class="form-control" placeholder="Enter teacher name" required>
            </div>
            <button class="btn btn-success w-100" type="submit">Add Teacher</button>
          </form>
        </div>
      </div>
    </div>
  </div>

  <!-- Tables -->
  <div class="row mt-5">
    <div class="col-md-6">
      <h5>📚 Student List</h5>
      <div class="mb-2">
        <button class="btn btn-outline-primary btn-sm" onclick="exportExcel('studentTable')">Export Excel</button>
        <button class="btn btn-outline-danger btn-sm" onclick="exportPDF('studentTable')">Export PDF</button>
      </div>
      <table class="table table-bordered" id="studentTable" style="display: none;">
        <thead class="table-light">
          <tr><th>#</th><th>Full Name</th><th>Student ID</th></tr>
        </thead>
        <tbody></tbody>
      </table>
    </div>

    <div class="col-md-6">
      <h5>📘 Teacher List</h5>
      <div class="mb-2">
        <button class="btn btn-outline-primary btn-sm" onclick="exportExcel('teacherTable')">Export Excel</button>
        <button class="btn btn-outline-danger btn-sm" onclick="exportPDF('teacherTable')">Export PDF</button>
      </div>
      <table class="table table-bordered" id="teacherTable" style="display: none;">
        <thead class="table-light">
          <tr><th>#</th><th>Full Name</th><th>Teacher ID</th></tr>
        </thead>
        <tbody></tbody>
      </table>
    </div>
  </div>
</div>

<script>
  let studentCount = 1, teacherCount = 1;
  let studentIdBase = 1001, teacherIdBase = 2001;

  // Add Student
  document.getElementById("studentForm").addEventListener("submit", function(e) {
    e.preventDefault();
    const name = document.getElementById("studentName").value.trim();
    if (!name) return alert("Please enter student name.");
    const id = studentIdBase++ + "std";
    const tbody = document.querySelector("#studentTable tbody");
    const row = `<tr><td>${studentCount++}</td><td>${name}</td><td>${id}</td></tr>`;
    tbody.innerHTML += row;
    document.getElementById("studentTable").style.display = "table";
    this.reset();
  });

  // Add Teacher
  document.getElementById("teacherForm").addEventListener("submit", function(e) {
    e.preventDefault();
    const name = document.getElementById("teacherName").value.trim();
    if (!name) return alert("Please enter teacher name.");
    const id = teacherIdBase++ + "tch";
    const tbody = document.querySelector("#teacherTable tbody");
    const row = `<tr><td>${teacherCount++}</td><td>${name}</td><td>${id}</td></tr>`;
    tbody.innerHTML += row;
    document.getElementById("teacherTable").style.display = "table";
    this.reset();
  });

  // Export Excel
  function exportExcel(tableId) {
    const table = document.getElementById(tableId);
    const wb = XLSX.utils.table_to_book(table, { sheet: tableId });
    XLSX.writeFile(wb, `${tableId}.xlsx`);
  }

  // Export PDF
  function exportPDF(tableId) {
    const { jsPDF } = window.jspdf;
    const doc = new jsPDF();
    doc.autoTable({ html: `#${tableId}` });
    doc.save(`${tableId}.pdf`);
  }
</script>

</body>
</html>
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Attendance Dashboard</title>
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/css/bootstrap.min.css" rel="stylesheet"/>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/xlsx/0.18.5/xlsx.full.min.js"></script>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf-autotable/3.5.25/jspdf.plugin.autotable.min.js"></script>
  <style>
    body { background-color: #f8f9fa; }
    .card { box-shadow: 0 0 10px rgba(0,0,0,0.05); }
  </style>
</head>
<body>

<div class="container py-5">
  <h2 class="text-center mb-4">🎓 Attendance Dashboard (Students & Teachers)</h2>

  <!-- Attendance Form -->
  <div class="card mb-4">
    <div class="card-header bg-primary text-white">➕ Mark Attendance</div>
    <div class="card-body">
      <form id="attendanceForm">
        <div class="row g-3 align-items-end">
          <div class="col-md-4">
            <label class="form-label">Full Name</label>
            <input type="text" id="fullName" class="form-control" placeholder="Enter name" required>
          </div>
          <div class="col-md-3">
            <label class="form-label">Role</label>
            <select id="role" class="form-select" required>
              <option value="">Select Role</option>
              <option value="Student">Student</option>
              <option value="Teacher">Teacher</option>
            </select>
          </div>
          <div class="col-md-3">
            <label class="form-label">Status</label>
            <select id="status" class="form-select" required>
              <option value="">Select Status</option>
              <option value="Present">✅ Present</option>
              <option value="Absent">❌ Absent</option>
            </select>
          </div>
          <div class="col-md-2">
            <button class="btn btn-success w-100" type="submit">Mark</button>
          </div>
        </div>
      </form>
    </div>
  </div>

  <!-- Export Buttons -->
  <div class="mb-3">
    <button class="btn btn-outline-success btn-sm" onclick="exportExcel()">Export to Excel</button>
    <button class="btn btn-outline-danger btn-sm" onclick="exportPDF()">Export to PDF</button>
  </div>

  <!-- Attendance Table -->
  <table class="table table-bordered" id="attendanceTable" style="display: none;">
    <thead class="table-dark">
      <tr><th>#</th><th>Name</th><th>Role</th><th>ID</th><th>Status</th><th>Date</th></tr>
    </thead>
    <tbody id="attendanceBody"></tbody>
  </table>
</div>

<script>
  let counter = 1;
  let studentId = 1000;
  let teacherId = 2000;

  document.getElementById("attendanceForm").addEventListener("submit", function(e) {
    e.preventDefault();

    const name = document.getElementById("fullName").value.trim();
    const role = document.getElementById("role").value;
    const status = document.getElementById("status").value;
    const date = new Date().toLocaleDateString();

    if (!name || !role || !status) {
      alert("❌ Please fill all fields.");
      return;
    }

    // Generate Auto ID
    let id = (role === "Student" ? studentId++ : teacherId++) + (role === "Student" ? "STD" : "TCH");

    const table = document.getElementById("attendanceTable");
    const tbody = document.getElementById("attendanceBody");

    const row = document.createElement("tr");
    row.innerHTML = `
      <td>${counter++}</td>
      <td>${name}</td>
      <td>${role}</td>
      <td>${id}</td>
      <td>${status}</td>
      <td>${date}</td>
    `;
    tbody.appendChild(row);
    table.style.display = "table";

    this.reset();
  });

  function exportExcel() {
    const table = document.getElementById("attendanceTable");
    const wb = XLSX.utils.table_to_book(table, { sheet: "Attendance" });
    XLSX.writeFile(wb, "attendance.xlsx");
  }

  function exportPDF() {
    const { jsPDF } = window.jspdf;
    const doc = new jsPDF();
    doc.autoTable({ html: "#attendanceTable" });
    doc.save("attendance.pdf");
  }
</script>

</body>
</html>
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <title>Login System (Fixed)</title>
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/css/bootstrap.min.css" rel="stylesheet" />
  <style>
    body { background: #f0f2f5; }
    .login-box { max-width: 400px; margin: 80px auto; padding: 30px; background: #fff; border-radius: 8px; box-shadow: 0 0 15px rgba(0,0,0,0.1); }
  </style>
</head>
<body>

<div class="login-box">
  <h3 class="text-center mb-4">🔐 Login System</h3>
  <form id="loginForm">
    <div class="mb-3">
      <label class="form-label">Username</label>
      <input type="text" id="username" class="form-control" placeholder="Enter username" required />
    </div>
    <div class="mb-3">
      <label class="form-label">Password</label>
      <input type="password" id="password" class="form-control" placeholder="Enter password" required />
    </div>
    <button type="submit" class="btn btn-primary w-100">Login</button>
  </form>

  <div id="dashboard" class="mt-4" style="display:none;">
    <h5>Welcome, <span id="role"></span> ✅</h5>
    <p>You have successfully logged in.</p>
    <button class="btn btn-danger btn-sm" onclick="logout()">Logout</button>
  </div>
</div>

<script>
  const users = [
    { username: "admin", password: "admin123", role: "Admin" },
    { username: "teacher", password: "teach123", role: "Teacher" },
    { username: "student", password: "stud123", role: "Student" }
  ];

  const form = document.getElementById("loginForm");

  form.addEventListener("submit", function(e) {
    e.preventDefault();
    const username = document.getElementById("username").value.trim();
    const password = document.getElementById("password").value.trim();

    const user = users.find(u => u.username === username && u.password === password);

    if (user) {
      form.style.display = "none";
      document.getElementById("dashboard").style.display = "block";
      document.getElementById("role").textContent = user.role;
    } else {
      alert("❌ Invalid username or password!");
    }
  });

  function logout() {
    form.reset();
    form.style.display = "block";
    document.getElementById("dashboard").style.display = "none";
  }
</script>

</body>
</html>


