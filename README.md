<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Faculty Resource Portal | DepEd Instructional Hub</title>
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.2/dist/css/bootstrap.min.css" rel="stylesheet">
    <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/bootstrap-icons@1.11.1/font/bootstrap-icons.css">
    <style>
        body {
            background-color: #f4f6f9;
            font-family: 'Segoe UI', system-ui, -apple-system, sans-serif;
        }
        .hero-header {
            background: linear-gradient(135deg, #0d47a1 0%, #1565c0 100%);
            color: white;
        }
        .doc-card {
            border: none;
            border-radius: 12px;
            transition: transform 0.18s ease, box-shadow 0.18s ease;
        }
        .doc-card:hover {
            transform: translateY(-4px);
            box-shadow: 0 10px 20px rgba(0, 0, 0, 0.08) !important;
        }
        .nav-pills .nav-link {
            border-radius: 8px;
            font-weight: 500;
            transition: all 0.15s ease;
        }
        .nav-pills .nav-link.active {
            background-color: #0d47a1;
        }
        .badge-wlp { background-color: #e3f2fd; color: #0d47a1; }
        .badge-bow { background-color: #e8f5e9; color: #1b5e20; }
        .badge-tq  { background-color: #fff3e0; color: #e65100; }
        .badge-tos { background-color: #f3e5f5; color: #4a148c; }
    </style>
</head>
<body>

    <!-- Top Header -->
    <header class="hero-header py-4 mb-4 shadow-sm">
        <div class="container">
            <div class="d-flex flex-column flex-md-row justify-content-between align-items-md-center gap-3">
                <div>
                    <h3 class="fw-bold mb-1">
                        <i class="bi bi-journal-bookmark-fill me-2"></i>Faculty Instructional Resource Portal
                    </h3>
                    <p class="mb-0 text-white-50">
                        Weekly Lesson Plans &bull; Budget of Work &bull; Test Questionnaires &bull; Table of Specifications
                    </p>
                </div>
                <div class="d-flex align-items-center gap-2">
                    <span id="syncStatusBadge" class="badge bg-light text-dark px-3 py-2">
                        <i class="bi bi-database-check text-success me-1"></i> Demo Data Loaded
                    </span>
                    <button class="btn btn-outline-light btn-sm" data-bs-toggle="modal" data-bs-target="#sheetConfigModal">
                        <i class="bi bi-gear-fill me-1"></i> Sheet Setup
                    </button>
                </div>
            </div>
        </div>
    </header>

    <main class="container mb-5">
        <!-- Category Tabs -->
        <ul class="nav nav-pills mb-3 gap-2" id="categoryTabs">
            <li class="nav-item">
                <button class="nav-link active" onclick="setCategory('All', this)">
                    All Materials <span class="badge bg-secondary ms-1" id="count-All">0</span>
                </button>
            </li>
            <li class="nav-item">
                <button class="nav-link bg-white text-dark border" onclick="setCategory('Weekly Lesson Plan', this)">
                    Weekly Lesson Plans <span class="badge bg-light text-dark border ms-1" id="count-WLP">0</span>
                </button>
            </li>
            <li class="nav-item">
                <button class="nav-link bg-white text-dark border" onclick="setCategory('Budget of Work', this)">
                    Budget of Work <span class="badge bg-light text-dark border ms-1" id="count-BOW">0</span>
                </button>
            </li>
            <li class="nav-item">
                <button class="nav-link bg-white text-dark border" onclick="setCategory('Test Questionnaire', this)">
                    Test Questionnaires <span class="badge bg-light text-dark border ms-1" id="count-TQ">0</span>
                </button>
            </li>
            <li class="nav-item">
                <button class="nav-link bg-white text-dark border" onclick="setCategory('Table of Specifications', this)">
                    Table of Specifications <span class="badge bg-light text-dark border ms-1" id="count-TOS">0</span>
                </button>
            </li>
        </ul>

        <!-- Search & Filter Bar -->
        <div class="card border-0 shadow-sm mb-4" style="border-radius: 12px;">
            <div class="card-body">
                <div class="row g-2">
                    <div class="col-md-5">
                        <div class="input-group">
                            <span class="input-group-text bg-white border-end-0">
                                <i class="bi bi-search text-muted"></i>
                            </span>
                            <input type="text" id="searchInput" class="form-control border-start-0"
                                placeholder="Search topic, subject, or file title..." oninput="renderFiles()">
                        </div>
                    </div>
                    <div class="col-md-3">
                        <select id="gradeFilter" class="form-select" onchange="renderFiles()">
                            <option value="All">All Grade Levels</option>
                            <option value="Grade 7">Grade 7</option>
                            <option value="Grade 8">Grade 8</option>
                            <option value="Grade 9">Grade 9</option>
                            <option value="Grade 10">Grade 10</option>
                            <option value="Grade 11">Grade 11</option>
                            <option value="Grade 12">Grade 12</option>
                        </select>
                    </div>
                    <div class="col-md-2">
                        <select id="termFilter" class="form-select" onchange="renderFiles()">
                            <option value="All">All Terms</option>
                            <option value="Term 1">Term 1</option>
                            <option value="Term 2">Term 2</option>
                            <option value="Term 3">Term 3</option>
                            <option value="Whole Year">Whole Year</option>
                        </select>
                    </div>
                    <div class="col-md-2 d-grid">
                        <button class="btn btn-outline-secondary" onclick="resetFilters()">
                            <i class="bi bi-arrow-counterclockwise me-1"></i> Reset
                        </button>
                    </div>
                </div>
            </div>
        </div>

        <!-- Results Header -->
        <div class="d-flex justify-content-between align-items-center mb-3">
            <span class="text-muted small fw-semibold" id="resultCount">Showing 0 documents</span>
            <button class="btn btn-sm btn-link text-decoration-none" onclick="loadData()">
                <i class="bi bi-arrow-repeat me-1"></i> Refresh Sheet Data
            </button>
        </div>

        <!-- Resource Cards Grid -->
        <div class="row g-3" id="fileGrid"></div>
    </main>

    <!-- Google Sheet Setup Modal -->
    <div class="modal fade" id="sheetConfigModal" tabindex="-1" aria-hidden="true">
        <div class="modal-dialog modal-lg">
            <div class="modal-content">
                <div class="modal-header bg-primary text-white">
                    <h5 class="modal-title"><i class="bi bi-file-earmark-spreadsheet me-2"></i>Connect Live Google Sheet</h5>
                    <button type="button" class="btn-close btn-close-white" data-bs-dismiss="modal"></button>
                </div>
                <div class="modal-body">
                    <ol class="small mb-3">
                        <li class="mb-1">Download the CSV template below and upload it to <strong>Google Sheets</strong>.</li>
                        <li class="mb-1">Ensure Row 1 headers are: <code>title</code>, <code>category</code>, <code>grade</code>, <code>subject</code>, <code>term</code>, <code>fileType</code>, <code>downloadUrl</code>.</li>
                        <li class="mb-1">In Google Sheets, click <strong>File &rarr; Share &rarr; Publish to web</strong>, choose <strong>Comma-separated values (.csv)</strong>, and copy the link.</li>
                        <li>Paste your link below to test it live, or paste it into <code>SHEET_CSV_URL</code> inside <code>index.html</code> on GitHub.</li>
                    </ol>

                    <div class="mb-3">
                        <button class="btn btn-success btn-sm" onclick="downloadCsvTemplate()">
                            <i class="bi bi-download me-1"></i> Download Google Sheet CSV Template
                        </button>
                    </div>

                    <div class="mb-3">
                        <label class="form-label fw-semibold small">Test Google Sheet Published CSV URL:</label>
                        <div class="input-group">
                            <input type="url" id="testCsvUrlInput" class="form-control" placeholder="https://docs.google.com/spreadsheets/d/e/.../pub?output=csv">
                            <button class="btn btn-primary" onclick="applyTestUrl()">Connect & Test</button>
                        </div>
                    </div>

                    <div class="bg-light p-2 rounded border small">
                        <strong>Permanent GitHub Code Snippet:</strong>
                        <pre class="mb-0 mt-1 text-primary" id="codeSnippetPreview">const SHEET_CSV_URL = "";</pre>
                    </div>
                </div>
            </div>
        </div>
    </div>

    <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.2/dist/js/bootstrap.bundle.min.js"></script>
    <script>
        // =========================================================================
        // PASTE YOUR PUBLISHED GOOGLE SHEET CSV LINK BETWEEN THE QUOTES BELOW:
        // =========================================================================
        let SHEET_CSV_URL = "https://docs.google.com/spreadsheets/d/e/2PACX-1vRi8m8dcp6aSPmXHiPBkMbfSkP9eMF9jVG_17hGGzpSo6zJdd-8_bV4pFMugErLAIQrv37YqTJ7ftkn/pub?output=csv";

        // Built-in fallback sample data (displays until SHEET_CSV_URL is connected)
        const fallbackResources = [
            {
                title: "Term 1 Week 1-2: Quadratic Equations DLL",
                category: "Weekly Lesson Plan",
                grade: "Grade 9",
                subject: "Mathematics",
                term: "Term 1",
                fileType: "DOCX",
                downloadUrl: "#"
            },
            {
                title: "Grade 8 Science Full-Year Budget of Work",
                category: "Budget of Work",
                grade: "Grade 8",
                subject: "Science",
                term: "Whole Year",
                fileType: "XLSX",
                downloadUrl: "#"
            },
            {
                title: "Term 1 Summative Examination - English 10",
                category: "Test Questionnaire",
                grade: "Grade 10",
                subject: "English",
                term: "Term 1",
                fileType: "PDF",
                downloadUrl: "#"
            },
            {
                title: "Term 1 Summative Exam TOS (Bloom's Taxonomy)",
                category: "Table of Specifications",
                grade: "Grade 10",
                subject: "English",
                term: "Term 1",
                fileType: "XLSX",
                downloadUrl: "#"
            },
            {
                title: "Term 2 Week 3: Philippine History & Governance",
                category: "Weekly Lesson Plan",
                grade: "Grade 7",
                subject: "Araling Panlipunan",
                term: "Term 2",
                fileType: "DOCX",
                downloadUrl: "#"
            },
            {
                title: "Term 3 Comprehensive Assessment & Answer Key",
                category: "Test Questionnaire",
                grade: "Grade 11",
                subject: "General Mathematics",
                term: "Term 3",
                fileType: "PDF",
                downloadUrl: "#"
            }
        ];

        let allResources = [];
        let activeCategory = 'All';

        // Robust CSV Parser (handles commas inside quoted titles)
        function parseCSV(csvText) {
            const lines = csvText.trim().split(/\r?\n/);
            if (lines.length < 2) return [];

            const headers = lines[0].split(',').map(h => h.trim().replace(/^"|"$/g, ''));
            const items = [];

            for (let i = 1; i < lines.length; i++) {
                const row = [];
                let current = '';
                let inQuotes = false;

                for (let char of lines[i]) {
                    if (char === '"') {
                        inQuotes = !inQuotes;
                    } else if (char === ',' && !inQuotes) {
                        row.push(current.trim());
                        current = '';
                    } else {
                        current += char;
                    }
                }
                row.push(current.trim());

                if (row.length >= 6 && row[0]) {
                    const obj = {};
                    headers.forEach((header, idx) => {
                        obj[header] = (row[idx] || '').replace(/^"|"$/g, '');
                    });
                    items.push(obj);
                }
            }
            return items;
        }

        async function loadData() {
            const savedUrl = localStorage.getItem('teacherPortalSheetUrl');
            const activeUrl = SHEET_CSV_URL || savedUrl || "";
            const statusBadge = document.getElementById('syncStatusBadge');

            if (!activeUrl) {
                allResources = fallbackResources;
                statusBadge.innerHTML = `<i class="bi bi-info-circle-fill text-primary me-1"></i> Sample Preview Mode`;
                updateCategoryCounts();
                renderFiles();
                return;
            }

            try {
                statusBadge.innerHTML = `<i class="bi bi-arrow-repeat text-warning me-1"></i> Syncing Google Sheet...`;
                const response = await fetch(activeUrl);
                if (!response.ok) throw new Error("Network response was not ok");
                const csvText = await response.text();
                const parsed = parseCSV(csvText);

                if (parsed.length > 0) {
                    allResources = parsed;
                    statusBadge.innerHTML = `<i class="bi bi-check-circle-fill text-success me-1"></i> Live Google Sheet Connected`;
                } else {
                    throw new Error("No valid rows found");
                }
            } catch (err) {
                console.error(err);
                allResources = fallbackResources;
                statusBadge.innerHTML = `<i class="bi bi-exclamation-triangle-fill text-danger me-1"></i> Sheet Error (Showing Sample)`;
            }

            updateCategoryCounts();
            renderFiles();
        }

        function updateCategoryCounts() {
            document.getElementById('count-All').textContent = allResources.length;
            document.getElementById('count-WLP').textContent = allResources.filter(r => r.category === 'Weekly Lesson Plan').length;
            document.getElementById('count-BOW').textContent = allResources.filter(r => r.category === 'Budget of Work').length;
            document.getElementById('count-TQ').textContent = allResources.filter(r => r.category === 'Test Questionnaire').length;
            document.getElementById('count-TOS').textContent = allResources.filter(r => r.category === 'Table of Specifications').length;
        }

        function setCategory(category, btn) {
            activeCategory = category;
            document.querySelectorAll('#categoryTabs .nav-link').forEach(el => {
                el.classList.remove('active');
                el.classList.add('bg-white', 'text-dark', 'border');
            });
            btn.classList.add('active');
            btn.classList.remove('bg-white', 'text-dark', 'border');
            renderFiles();
        }

        function resetFilters() {
            document.getElementById('searchInput').value = '';
            document.getElementById('gradeFilter').value = 'All';
            document.getElementById('termFilter').value = 'All';
            renderFiles();
        }

        function getBadgeClass(category) {
            switch (category) {
                case 'Weekly Lesson Plan': return 'badge-wlp';
                case 'Budget of Work': return 'badge-bow';
                case 'Test Questionnaire': return 'badge-tq';
                case 'Table of Specifications': return 'badge-tos';
                default: return 'bg-secondary text-white';
            }
        }

        function renderFiles() {
            const search = document.getElementById('searchInput').value.toLowerCase();
            const grade = document.getElementById('gradeFilter').value;
            const term = document.getElementById('termFilter').value;
            const grid = document.getElementById('fileGrid');

            const filtered = allResources.filter(item => {
                const matchCat = activeCategory === 'All' || item.category === activeCategory;
                const matchGrade = grade === 'All' || item.grade === grade;
                const matchTerm = term === 'All' || item.term === term;
                const matchSearch = (item.title || '').toLowerCase().includes(search) ||
                                    (item.subject || '').toLowerCase().includes(search);
                return matchCat && matchGrade && matchTerm && matchSearch;
            });

            document.getElementById('resultCount').textContent = `Showing ${filtered.length} document(s)`;
            grid.innerHTML = '';

            if (filtered.length === 0) {
                grid.innerHTML = `
                    <div class="col-12 text-center py-5">
                        <i class="bi bi-folder2-open display-4 text-muted"></i>
                        <p class="text-muted mt-2">No matching instructional materials found for the selected filters.</p>
                    </div>`;
                return;
            }

            filtered.forEach(item => {
                const badgeClass = getBadgeClass(item.category);
                const isSample = !item.downloadUrl || item.downloadUrl === '#';
                const btnAction = isSample
                    ? `onclick="alert('Sample Preview: Connect your Google Sheet URL in Sheet Setup to enable live file downloads.')"`
                    : `href="${item.downloadUrl}" target="_blank" rel="noopener"`;

                grid.innerHTML += `
                    <div class="col-md-6 col-lg-4">
                        <div class="card doc-card h-100 shadow-sm">
                            <div class="card-body d-flex flex-column justify-content-between">
                                <div>
                                    <div class="d-flex justify-content-between align-items-center mb-2">
                                        <span class="badge ${badgeClass} px-2 py-1">${item.category}</span>
                                        <span class="badge bg-light text-secondary border">${item.fileType || 'FILE'}</span>
                                    </div>
                                    <h6 class="card-title fw-bold text-dark mb-1">${item.title}</h6>
                                    <p class="text-muted small mb-3">
                                        <i class="bi bi-mortarboard me-1"></i>${item.grade} &bull; ${item.subject} &bull; <strong>${item.term}</strong>
                                    </p>
                                </div>
                                <a ${btnAction} class="btn btn-primary btn-sm w-100 fw-medium">
                                    <i class="bi bi-cloud-arrow-down-fill me-1"></i> Download Material
                                </a>
                            </div>
                        </div>
                    </div>
                `;
            });
        }

        function applyTestUrl() {
            const url = document.getElementById('testCsvUrlInput').value.trim();
            if (!url) return;
            localStorage.setItem('teacherPortalSheetUrl', url);
            document.getElementById('codeSnippetPreview').textContent = `let SHEET_CSV_URL = "${url}";`;
            loadData();
        }

        function downloadCsvTemplate() {
            const csvContent = [
                "title,category,grade,subject,term,fileType,downloadUrl",
                '"Term 1 Week 1: Quadratic Equations DLL","Weekly Lesson Plan","Grade 9","Mathematics","Term 1","DOCX","https://drive.google.com/drive/folders/1p98u2UQjLUcq_VR8-3pJZPULppDRcmX7?usp=sharing"',
                '"Grade 8 Science Full-Year Budget of Work","Budget of Work","Grade 8","Science","Whole Year","XLSX","[https://drive.google.com/your-file-link](https://drive.google.com/drive/folders/1p98u2UQjLUcq_VR8-3pJZPULppDRcmX7?usp=sharing
)"',
                '"Term 1 Summative Exam - English 10","Test Questionnaire","Grade 10","English","Term 1","PDF","[https://drive.google.com/your-file-link](https://drive.google.com/drive/folders/1p98u2UQjLUcq_VR8-3pJZPULppDRcmX7?usp=sharing
)"',
                '"Term 1 Summative Exam TOS","Table of Specifications","Grade 10","English","Term 1","XLSX","[https://drive.google.com/your-file-link](https://drive.google.com/drive/folders/1p98u2UQjLUcq_VR8-3pJZPULppDRcmX7?usp=sharing
)"'
            ].join("\n");

            const blob = new Blob([csvContent], { type: 'text/csv;charset=utf-8;' });
            const link = document.createElement("a");
            link.href = URL.createObjectURL(blob);
            link.download = "deped_teacher_portal_template.csv";
            link.click();
        }

        // Initialize on page load
        loadData();
    </script>
</body>
</html>
