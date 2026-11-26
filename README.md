<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Faculty Instructional Resource Portal</title>
    <!-- Google Fonts: Inter -->
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&display=swap" rel="stylesheet">
    <!-- Bootstrap 5 CSS -->
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.2/dist/css/bootstrap.min.css" rel="stylesheet">
    <!-- Bootstrap Icons -->
    <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/bootstrap-icons@1.11.3/font/bootstrap-icons.min.css">
    
    <style>
        :root {
            --royal-primary: #0f3d91;
            --royal-secondary: #1d5bbf;
            --surface-bg: #f4f7fb;
        }

        body {
            font-family: 'Inter', sans-serif;
            background-color: var(--surface-bg);
            color: #1e293b;
        }

        /* Royal Blue Academic Header */
        .academic-header {
            background: linear-gradient(135deg, var(--royal-primary) 0%, var(--royal-secondary) 100%);
            color: #ffffff;
            border-bottom: 4px solid #f59e0b;
        }

        /* Live Sync Status Pill */
        .sync-badge {
            background-color: rgba(255, 255, 255, 0.14);
            backdrop-filter: blur(6px);
            border: 1px solid rgba(255, 255, 255, 0.25);
            font-size: 0.82rem;
        }

        .pulse-dot {
            width: 8px;
            height: 8px;
            border-radius: 50%;
            display: inline-block;
            animation: pulseAnim 2s infinite;
        }

        @keyframes pulseAnim {
            0% { box-shadow: 0 0 0 0 rgba(74, 222, 128, 0.7); }
            70% { box-shadow: 0 0 0 6px rgba(74, 222, 128, 0); }
            100% { box-shadow: 0 0 0 0 rgba(74, 222, 128, 0); }
        }

        /* Interactive Category Pills */
        .category-pills .nav-link {
            border-radius: 50rem;
            font-weight: 500;
            font-size: 0.9rem;
            color: #334155;
            background-color: #ffffff;
            border: 1px solid #cbd5e1;
            padding: 0.5rem 1.1rem;
            transition: all 0.2s ease;
        }

        .category-pills .nav-link:hover {
            border-color: var(--royal-secondary);
            color: var(--royal-secondary);
            transform: translateY(-1px);
        }

        .category-pills .nav-link.active {
            background-color: var(--royal-primary);
            border-color: var(--royal-primary);
            color: #ffffff;
            box-shadow: 0 4px 10px rgba(15, 61, 145, 0.25);
        }

        .category-pills .nav-link.active .badge {
            background-color: #ffffff !important;
            color: var(--royal-primary) !important;
        }

        /* Resource Cards */
        .resource-card {
            border: 1px solid #e2e8f0;
            border-radius: 0.85rem;
            transition: transform 0.2s ease, box-shadow 0.2s ease, border-color 0.2s ease;
            background: #ffffff;
        }

        .resource-card:hover {
            transform: translateY(-4px);
            box-shadow: 0 12px 24px -8px rgba(15, 61, 145, 0.15);
            border-color: #93c5fd;
        }

        /* Document Type Badges */
        .badge-wlp { background-color: #dbeafe; color: #1e40af; }
        .badge-bow { background-color: #dcfce7; color: #166534; }
        .badge-tq  { background-color: #fef3c7; color: #92400e; }
        .badge-tos { background-color: #f3e8ff; color: #6b21a8; }

        /* Format Pill */
        .format-badge {
            font-size: 0.72rem;
            letter-spacing: 0.04em;
            background-color: #f1f5f9;
            color: #475569;
            border: 1px solid #cbd5e1;
        }

        /* Toast Notification */
        .custom-toast {
            position: fixed;
            bottom: 1.5rem;
            right: 1.5rem;
            z-index: 1090;
            min-width: 280px;
        }
    </style>
</head>
<body>

    <!-- =====================================================================
         1. ROYAL BLUE ACADEMIC HEADER
         ===================================================================== -->
    <header class="academic-header py-4 shadow-sm">
        <div class="container">
            <div class="row align-items-center gy-3">
                <div class="col-lg-8">
                    <div class="d-flex align-items-center gap-3">
                        <div class="bg-white text-primary rounded-3 p-3 shadow-sm d-none d-sm-flex align-items-center justify-content-center">
                            <i class="bi bi-mortarboard-fill fs-2" style="color: var(--royal-primary);"></i>
                        </div>
                        <div>
                            <div class="d-flex flex-wrap align-items-center gap-2 mb-1">
                                <span class="badge bg-warning text-dark fw-semibold rounded-pill px-3">
                                    <i class="bi bi-calendar3 me-1"></i> SY 2026–2027
                                </span>
                                <!-- Live Sync Status Indicator -->
                                <span id="syncStatusBadge" class="badge sync-badge rounded-pill px-3 py-1 d-inline-flex align-items-center gap-2">
                                    <span id="syncDot" class="pulse-dot bg-warning"></span>
                                    <span id="syncStatusText">Built-in Sample Data Mode</span>
                                </span>
                            </div>
                            <h1 class="h3 fw-bold mb-1">Faculty Instructional Resource Portal</h1>
                            <p class="mb-0 text-white-50 small">
                                Open-Access Repository for Weekly Lesson Plans (DLL/DLP), Budgets of Work (BOW), Test Questionnaires &amp; Tables of Specifications (TOS)
                            </p>
                        </div>
                    </div>
                </div>
                <div class="col-lg-4 text-lg-end">
                    <button class="btn btn-light btn-sm fw-semibold rounded-pill px-3 py-2 shadow-sm" data-bs-toggle="modal" data-bs-target="#sheetSetupModal">
                        <i class="bi bi-filetype-csv text-success me-1"></i> Google Sheet Setup &amp; Tester
                    </button>
                </div>
            </div>
        </div>
    </header>

    <!-- =====================================================================
         2. MAIN INTERACTIVE PORTAL BODY
         ===================================================================== -->
    <main class="container my-4 pb-5">

        <!-- Interactive Category Navigation Pills -->
        <div class="d-flex justify-content-between align-items-center flex-wrap gap-2 mb-3">
            <ul class="nav nav-pills category-pills gap-2" id="categoryTabs" role="tablist">
                <li class="nav-item">
                    <button class="nav-link active" data-category="All" onclick="selectCategory('All', this)">
                        All Files <span class="badge bg-secondary ms-1 rounded-pill" id="count-All">0</span>
                    </button>
                </li>
                <li class="nav-item">
                    <button class="nav-link" data-category="Weekly Lesson Plan" onclick="selectCategory('Weekly Lesson Plan', this)">
                        <i class="bi bi-journal-text me-1"></i> Weekly Lesson Plans <span class="badge bg-light text-dark border ms-1 rounded-pill" id="count-WLP">0</span>
                    </button>
                </li>
                <li class="nav-item">
                    <button class="nav-link" data-category="Budget of Work" onclick="selectCategory('Budget of Work', this)">
                        <i class="bi bi-calendar2-week me-1"></i> Budget of Work <span class="badge bg-light text-dark border ms-1 rounded-pill" id="count-BOW">0</span>
                    </button>
                </li>
                <li class="nav-item">
                    <button class="nav-link" data-category="Test Questionnaire" onclick="selectCategory('Test Questionnaire', this)">
                        <i class="bi bi-file-earmark-check me-1"></i> Test Questionnaires <span class="badge bg-light text-dark border ms-1 rounded-pill" id="count-TQ">0</span>
                    </button>
                </li>
                <li class="nav-item">
                    <button class="nav-link" data-category="Table of Specifications" onclick="selectCategory('Table of Specifications', this)">
                        <i class="bi bi-table me-1"></i> Table of Specifications <span class="badge bg-light text-dark border ms-1 rounded-pill" id="count-TOS">0</span>
                    </button>
                </li>
            </ul>
        </div>

        <!-- Multi-Faceted Filtering & Sorting Card -->
        <div class="card border-0 shadow-sm rounded-4 mb-4">
            <div class="card-body p-3 p-md-4">
                <div class="row g-2 g-md-3">
                    <!-- Keyword Search -->
                    <div class="col-12 col-lg-4">
                        <label for="searchInput" class="form-label small fw-semibold text-muted mb-1">Search Materials</label>
                        <div class="input-group">
                            <span class="input-group-text bg-white border-end-0 text-muted"><i class="bi bi-search"></i></span>
                            <input type="text" id="searchInput" class="form-control border-start-0 ps-0" placeholder="Topic, subject, week (e.g., Week 1, Algebra)..." oninput="applyFilters()">
                        </div>
                    </div>

                    <!-- Grade Level Filter -->
                    <div class="col-6 col-md-3 col-lg-2">
                        <label for="gradeFilter" class="form-label small fw-semibold text-muted mb-1">Grade Level</label>
                        <select id="gradeFilter" class="form-select" onchange="applyFilters()">
                            <option value="All">All Grades</option>
                            <option value="Grade 7">Grade 7</option>
                            <option value="Grade 8">Grade 8</option>
                            <option value="Grade 9">Grade 9</option>
                            <option value="Grade 10">Grade 10</option>
                            <option value="Grade 11">Grade 11</option>
                            <option value="Grade 12">Grade 12</option>
                        </select>
                    </div>

                    <!-- Dynamic Subject Filter -->
                    <div class="col-6 col-md-3 col-lg-2">
                        <label for="subjectFilter" class="form-label small fw-semibold text-muted mb-1">Subject</label>
                        <select id="subjectFilter" class="form-select" onchange="applyFilters()">
                            <option value="All">All Subjects</option>
                        </select>
                    </div>

                    <!-- Quarter Filter -->
                    <div class="col-6 col-md-3 col-lg-2">
                        <label for="quarterFilter" class="form-label small fw-semibold text-muted mb-1">Quarter</label>
                        <select id="quarterFilter" class="form-select" onchange="applyFilters()">
                            <option value="All">All Quarters</option>
                            <option value="Quarter 1">Quarter 1</option>
                            <option value="Quarter 2">Quarter 2</option>
                            <option value="Quarter 3">Quarter 3</option>
                            <option value="Quarter 4">Quarter 4</option>
                            <option value="Whole Year">Whole Year</option>
                        </select>
                    </div>

                    <!-- Sort Dropdown -->
                    <div class="col-6 col-md-3 col-lg-2">
                        <label for="sortSelect" class="form-label small fw-semibold text-muted mb-1">Sort By</label>
                        <select id="sortSelect" class="form-select" onchange="applyFilters()">
                            <option value="newest">Newest First</option>
                            <option value="title">Title (A–Z)</option>
                            <option value="grade">Grade Level</option>
                        </select>
                    </div>
                </div>
            </div>
        </div>

        <!-- Results Toolbar & View Mode Toggle (Cards vs Table) -->
        <div class="d-flex flex-wrap justify-content-between align-items-center gap-2 mb-3">
            <div class="d-flex align-items-center gap-2">
                <span class="text-muted small fw-medium" id="resultCountText">Showing 0 resources</span>
                <button id="clearFiltersBtn" class="btn btn-link btn-sm text-decoration-none p-0 d-none" onclick="resetAllFilters()">
                    <i class="bi bi-x-circle me-1"></i>Clear filters
                </button>
            </div>

            <div class="btn-group btn-group-sm shadow-sm" role="group" aria-label="View toggle">
                <button type="button" id="btnCardView" class="btn btn-primary active" onclick="switchView('card')">
                    <i class="bi bi-grid-3x3-gap-fill me-1"></i> Cards
                </button>
                <button type="button" id="btnTableView" class="btn btn-outline-primary bg-white" onclick="switchView('table')">
                    <i class="bi bi-list-ul me-1"></i> Table
                </button>
            </div>
        </div>

        <!-- Loading Spinner -->
        <div id="loadingSpinner" class="text-center py-5 d-none">
            <div class="spinner-border text-primary" role="status"></div>
            <p class="text-muted small mt-2">Fetching instructional materials from Google Sheet...</p>
        </div>

        <!-- Card Grid Container -->
        <div id="cardViewContainer" class="row g-3"></div>

        <!-- Table List Container -->
        <div id="tableViewContainer" class="card border-0 shadow-sm rounded-4 overflow-hidden d-none">
            <div class="table-responsive">
                <table class="table table-hover align-middle mb-0">
                    <thead class="table-light small text-uppercase text-muted">
                        <tr>
                            <th class="ps-4">Document Type</th>
                            <th>Title &amp; Details</th>
                            <th>Grade &amp; Subject</th>
                            <th>Quarter</th>
                            <th>Date Added</th>
                            <th class="text-end pe-4">Actions</th>
                        </tr>
                    </thead>
                    <tbody id="tableBody"></tbody>
                </table>
            </div>
        </div>

    </main>

    <!-- =====================================================================
         3. RESOURCE PREVIEW DETAILS MODAL
         ===================================================================== -->
    <div class="modal fade" id="previewModal" tabindex="-1" aria-hidden="true">
        <div class="modal-dialog modal-dialog-centered">
            <div class="modal-content border-0 shadow rounded-4">
                <div class="modal-header border-bottom-0 pb-0">
                    <span id="modalCategoryBadge" class="badge rounded-pill px-3 py-1">Category</span>
                    <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
                </div>
                <div class="modal-body pt-3">
                    <h5 id="modalTitle" class="fw-bold mb-3">Resource Title</h5>
                    <div class="bg-light rounded-3 p-3 mb-3">
                        <div class="row g-2 small">
                            <div class="col-6">
                                <span class="text-muted d-block">Grade Level</span>
                                <strong id="modalGrade">-</strong>
                            </div>
                            <div class="col-6">
                                <span class="text-muted d-block">Subject Area</span>
                                <strong id="modalSubject">-</strong>
                            </div>
                            <div class="col-6">
                                <span class="text-muted d-block">Grading Quarter</span>
                                <strong id="modalQuarter">-</strong>
                            </div>
                            <div class="col-6">
                                <span class="text-muted d-block">File Format &amp; Date</span>
                                <strong id="modalMeta">-</strong>
                            </div>
                        </div>
                    </div>
                    <p class="small text-muted mb-0">
                        <i class="bi bi-info-circle me-1"></i> Clicking the download button below will open or download the file directly from your school's repository or Google Drive link.
                    </p>
                </div>
                <div class="modal-footer border-top-0">
                    <button type="button" class="btn btn-light rounded-pill px-3" data-bs-dismiss="modal">Close</button>
                    <a id="modalDownloadBtn" href="#" target="_blank" rel="noopener noreferrer" class="btn btn-primary rounded-pill px-4 fw-semibold">
                        <i class="bi bi-download me-1"></i> Download / Open File
                    </a>
                </div>
            </div>
        </div>
    </div>

    <!-- =====================================================================
         4. GOOGLE SHEET SETUP & LIVE TESTER MODAL
         ===================================================================== -->
    <div class="modal fade" id="sheetSetupModal" tabindex="-1" aria-hidden="true">
        <div class="modal-dialog modal-lg modal-dialog-centered modal-dialog-scrollable">
            <div class="modal-content border-0 shadow rounded-4">
                <div class="modal-header bg-light">
                    <h5 class="modal-title fw-bold">
                        <i class="bi bi-table text-success me-2"></i>Google Sheet Live Connector &amp; Setup
                    </h5>
                    <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
                </div>
                <div class="modal-body p-4">
                    
                    <!-- Step 1: Download CSV Template -->
                    <div class="mb-4">
                        <h6 class="fw-bold text-primary">Step 1: Prepare Your Google Sheet</h6>
                        <p class="small text-muted mb-2">
                            Create a Google Sheet with these exact 8 column headers in Row 1:
                            <code>title, category, grade, subject, quarter, fileType, fileUrl, dateAdded</code>
                        </p>
                        <button type="button" class="btn btn-outline-success btn-sm rounded-pill" onclick="downloadSampleCSV()">
                            <i class="bi bi-file-earmark-arrow-down me-1"></i> Download Sample CSV Template (teacher_portal_template.csv)
                        </button>
                    </div>

                    <!-- Step 2: Publish to Web -->
                    <div class="mb-4">
                        <h6 class="fw-bold text-primary">Step 2: Publish Google Sheet as CSV</h6>
                        <ol class="small text-muted ps-3 mb-0">
                            <li>In Google Sheets, click <strong>File &rarr; Share &rarr; Publish to web</strong>.</li>
                            <li>Change <em>Web page</em> to <strong>Comma-separated values (.csv)</strong> and click <strong>Publish</strong>.</li>
                            <li>Copy the generated <code>https://docs.google.com/spreadsheets/d/e/.../pub?output=csv</code> URL.</li>
                        </ol>
                    </div>

                    <!-- Step 3: Live Tester -->
                    <div class="mb-4 p-3 bg-light rounded-3 border">
                        <label for="testCsvUrlInput" class="form-label fw-bold small mb-1">Step 3: Test Your Google Sheet CSV URL Live</label>
                        <div class="input-group mb-2">
                            <input type="url" id="testCsvUrlInput" class="form-control form-control-sm" placeholder="https://docs.google.com/spreadsheets/d/e/.../pub?output=csv">
                            <button class="btn btn-primary btn-sm px-3 fw-semibold" type="button" onclick="testLiveGoogleSheet()">
                                <i class="bi bi-lightning-charge-fill me-1"></i> Load Live Data
                            </button>
                        </div>
                        <div id="testerFeedback" class="small"></div>
                    </div>

                    <!-- Step 4: Copy Config Code for GitHub -->
                    <div>
                        <div class="d-flex justify-content-between align-items-center mb-1">
                            <label class="form-label fw-bold small mb-0">Step 4: Save Permanently in GitHub (<code>index.html</code>)</label>
                            <button type="button" class="btn btn-outline-secondary btn-sm py-0 px-2" onclick="copyConfigSnippet()">
                                <i class="bi bi-clipboard me-1"></i> Copy Code
                            </button>
                        </div>
                        <p class="small text-muted mb-2">
                            Paste your URL into the <code>SHEET_CSV_URL</code> variable near the top of the <code>&lt;script&gt;</code> section in <code>index.html</code> so all teachers see your live sheet automatically:
                        </p>
                        <pre class="bg-dark text-light p-3 rounded-3 small mb-0"><code id="configCodeBlock">const SHEET_CSV_URL = "https://docs.google.com/spreadsheets/d/e/2PACX-1vRi8m8dcp6aSPmXHiPBkMbfSkP9eMF9jVG_17hGGzpSo6zJdd-8_bV4pFMugErLAIQrv37YqTJ7ftkn/pub?output=csv";</code></pre>
                    </div>

                </div>
                <div class="modal-footer bg-light">
                    <button type="button" class="btn btn-outline-danger btn-sm me-auto" onclick="revertToSampleData()">
                        Reset to Built-in Sample Data
                    </button>
                    <button type="button" class="btn btn-secondary rounded-pill px-4" data-bs-dismiss="modal">Done</button>
                </div>
            </div>
        </div>
    </div>

    <!-- =====================================================================
         5. TOAST NOTIFICATION CONTAINER
         ===================================================================== -->
    <div id="toastContainer" class="custom-toast"></div>

    <!-- Bootstrap 5 JS Bundle -->
    <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.2/dist/js/bootstrap.bundle.min.js"></script>

    <!-- =====================================================================
         6. PORTAL APPLICATION LOGIC
         ===================================================================== -->
    <script>
        /**
         * ===================================================================
         * CONFIGURATION: PASTE YOUR PUBLISHED GOOGLE SHEET CSV URL BELOW
         * Example:
         * const SHEET_CSV_URL = "https://docs.google.com/spreadsheets/d/e/2PACX-.../pub?output=csv";
         * Leave empty ("") to use the built-in sample data out of the box.
         * ===================================================================
         */
        const SHEET_CSV_URL = "";

        /**
         * Built-in Fallback Sample Data
         * Used automatically when SHEET_CSV_URL is empty or unreachable.
         */
        const SAMPLE_RESOURCES = [
            {
                title: "Q1 Week 1–2: Illustrating Quadratic Equations (DLL)",
                category: "Weekly Lesson Plan",
                grade: "Grade 9",
                subject: "Mathematics",
                quarter: "Quarter 1",
                fileType: "DOCX",
                fileUrl: "#sample-download-wlp-math9",
                dateAdded: "2026-08-10"
            },
            {
                title: "Science 8 Full-Year MELC-Based Budget of Work",
                category: "Budget of Work",
                grade: "Grade 8",
                subject: "Science",
                quarter: "Whole Year",
                fileType: "XLSX",
                fileUrl: "#sample-download-bow-sci8",
                dateAdded: "2026-08-05"
            },
            {
                title: "First Periodical Examination in English 10 (with Key)",
                category: "Test Questionnaire",
                grade: "Grade 10",
                subject: "English",
                quarter: "Quarter 1",
                fileType: "PDF",
                fileUrl: "#sample-download-tq-eng10",
                dateAdded: "2026-10-12"
            },
            {
                title: "Q1 Periodical Exam Table of Specifications (Bloom's Taxonomy)",
                category: "Table of Specifications",
                grade: "Grade 10",
                subject: "English",
                quarter: "Quarter 1",
                fileType: "XLSX",
                fileUrl: "#sample-download-tos-eng10",
                dateAdded: "2026-10-12"
            },
            {
                title: "Q2 Week 3: Kabihasnang Asyano at Pag-unlad (DLP)",
                category: "Weekly Lesson Plan",
                grade: "Grade 7",
                subject: "Araling Panlipunan",
                quarter: "Quarter 2",
                fileType: "DOCX",
                fileUrl: "#sample-download-wlp-ap7",
                dateAdded: "2026-11-02"
            },
            {
                title: "General Mathematics Core Subject Budget of Work",
                category: "Budget of Work",
                grade: "Grade 11",
                subject: "Mathematics",
                quarter: "Quarter 1",
                fileType: "PDF",
                fileUrl: "#sample-download-bow-genmath11",
                dateAdded: "2026-08-08"
            },
            {
                title: "Q2 Summative Assessment in Physical Science",
                category: "Test Questionnaire",
                grade: "Grade 12",
                subject: "Science",
                quarter: "Quarter 2",
                fileType: "DOCX",
                fileUrl: "#sample-download-tq-physci12",
                dateAdded: "2026-11-18"
            },
            {
                title: "Q2 Summative TOS & Item Placement Matrix",
                category: "Table of Specifications",
                grade: "Grade 12",
                subject: "Science",
                quarter: "Quarter 2",
                fileType: "XLSX",
                fileUrl: "#sample-download-tos-physci12",
                dateAdded: "2026-11-18"
            }
        ];

        // Global Application State
        let allResources = [];
        let activeCategory = "All";
        let currentView = "card"; // 'card' or 'table'

        // Category Styling Map
        const CATEGORY_META = {
            "Weekly Lesson Plan": { badgeClass: "badge-wlp", icon: "bi-journal-text" },
            "Budget of Work": { badgeClass: "badge-bow", icon: "bi-calendar2-week" },
            "Test Questionnaire": { badgeClass: "badge-tq", icon: "bi-file-earmark-check" },
            "Table of Specifications": { badgeClass: "badge-tos", icon: "bi-table" }
        };

        // Initialize Portal on Page Load
        document.addEventListener("DOMContentLoaded", () => {
            const savedTestUrl = localStorage.getItem("teacher_portal_test_csv_url");
            const activeUrl = SHEET_CSV_URL.trim() || savedTestUrl || "";

            if (activeUrl) {
                document.getElementById("testCsvUrlInput").value = activeUrl;
                updateConfigSnippet(activeUrl);
                loadFromGoogleSheet(activeUrl, false);
            } else {
                loadBuiltInSampleData();
            }
        });

        /**
         * Loads built-in fallback data into the portal
         */
        function loadBuiltInSampleData() {
            allResources = [...SAMPLE_RESOURCES];
            setSyncStatus("sample", "Built-in Sample Data Mode");
            populateSubjectDropdown();
            updateCategoryCounts();
            applyFilters();
        }

        /**
         * Fetches and parses CSV data from a published Google Sheet URL
         */
        async function loadFromGoogleSheet(csvUrl, isManualTest = false) {
            const spinner = document.getElementById("loadingSpinner");
            const feedback = document.getElementById("testerFeedback");

            spinner.classList.remove("d-none");
            if (isManualTest) {
                feedback.innerHTML = `<span class="text-primary"><i class="bi bi-arrow-repeat me-1"></i>Connecting to Google Sheet...</span>`;
            }

            try {
                // Append cache-busting timestamp so teachers always get latest updates
                const separator = csvUrl.includes("?") ? "&" : "?";
                const response = await fetch(`${csvUrl}${separator}t=${Date.now()}`);

                if (!response.ok) {
                    throw new Error(`HTTP Error ${response.status}: Unable to fetch CSV.`);
                }

                const csvText = await response.text();
                const parsedData = parseCSVToResources(csvText);

                if (parsedData.length === 0) {
                    throw new Error("CSV loaded, but no valid rows were found. Check column headers.");
                }

                allResources = parsedData;
                setSyncStatus("live", `Live Google Sheet Connected (${allResources.length} files)`);
                populateSubjectDropdown();
                updateCategoryCounts();
                applyFilters();

                if (isManualTest) {
                    localStorage.setItem("teacher_portal_test_csv_url", csvUrl);
                    updateConfigSnippet(csvUrl);
                    feedback.innerHTML = `<span class="text-success fw-semibold"><i class="bi bi-check-circle-fill me-1"></i>Success! Loaded ${allResources.length} resources from your Google Sheet.</span>`;
                    showToast("Google Sheet connected! Portal updated with live resources.", "success");
                }
            } catch (error) {
                console.error("Google Sheet Sync Error:", error);
                if (isManualTest) {
                    feedback.innerHTML = `<span class="text-danger fw-semibold"><i class="bi bi-exclamation-triangle-fill me-1"></i>Error: ${escapeHtml(error.message)}</span>`;
                } else {
                    // Fallback gracefully if configured URL fails
                    loadBuiltInSampleData();
                    showToast("Could not reach Google Sheet. Loaded fallback sample data.", "warning");
                }
            } finally {
                spinner.classList.add("d-none");
            }
        }

        /**
         * Robust RFC-4180 CSV Parser handling quoted fields with commas
         */
        function parseCSVToResources(csvText) {
            const rows = [];
            let currentRow = [];
            let currentVal = "";
            let insideQuotes = false;

            for (let i = 0; i < csvText.length; i++) {
                const char = csvText[i];
                const nextChar = csvText[i + 1];

                if (char === '"') {
                    if (insideQuotes && nextChar === '"') {
                        currentVal += '"';
                        i++;
                    } else {
                        insideQuotes = !insideQuotes;
                    }
                } else if (char === "," && !insideQuotes) {
                    currentRow.push(currentVal.trim());
                    currentVal = "";
                } else if ((char === "\r" || char === "\n") && !insideQuotes) {
                    if (char === "\r" && nextChar === "\n") i++;
                    currentRow.push(currentVal.trim());
                    if (currentRow.some(cell => cell !== "")) {
                        rows.push(currentRow);
                    }
                    currentRow = [];
                    currentVal = "";
                } else {
                    currentVal += char;
                }
            }
            if (currentVal !== "" || currentRow.length > 0) {
                currentRow.push(currentVal.trim());
                if (currentRow.some(cell => cell !== "")) rows.push(currentRow);
            }

            if (rows.length < 2) return [];

            // Map header indexes case-insensitively
            const headers = rows[0].map(h => h.toLowerCase().replace(/\s+/g, ""));
            const idx = {
                title: headers.indexOf("title"),
                category: headers.indexOf("category"),
                grade: headers.indexOf("grade"),
                subject: headers.indexOf("subject"),
                quarter: headers.indexOf("quarter"),
                fileType: headers.indexOf("filetype"),
                fileUrl: headers.indexOf("fileurl"),
                dateAdded: headers.indexOf("dateadded")
            };

            const resources = [];
            for (let r = 1; r < rows.length; r++) {
                const cols = rows[r];
                const title = idx.title > -1 ? cols[idx.title] : "";
                if (!title) continue;

                resources.push({
                    title: title,
                    category: (idx.category > -1 && cols[idx.category]) ? cols[idx.category] : "Weekly Lesson Plan",
                    grade: (idx.grade > -1 && cols[idx.grade]) ? cols[idx.grade] : "Grade 7",
                    subject: (idx.subject > -1 && cols[idx.subject]) ? cols[idx.subject] : "General",
                    quarter: (idx.quarter > -1 && cols[idx.quarter]) ? cols[idx.quarter] : "Quarter 1",
                    fileType: (idx.fileType > -1 && cols[idx.fileType]) ? cols[idx.fileType].toUpperCase() : "FILE",
                    fileUrl: (idx.fileUrl > -1 && cols[idx.fileUrl]) ? cols[idx.fileUrl] : "#",
                    dateAdded: (idx.dateAdded > -1 && cols[idx.dateAdded]) ? cols[idx.dateAdded] : "2026-01-01"
                });
            }
            return resources;
        }

        /**
         * Updates header sync status pill
         */
        function setSyncStatus(mode, text) {
            const dot = document.getElementById("syncDot");
            const label = document.getElementById("syncStatusText");
            label.textContent = text;
            if (mode === "live") {
                dot.className = "pulse-dot bg-success";
            } else {
                dot.className = "pulse-dot bg-warning";
            }
        }

        /**
         * Dynamically populates the Subject dropdown from loaded resources
         */
        function populateSubjectDropdown() {
            const subjectSelect = document.getElementById("subjectFilter");
            const subjects = [...new Set(allResources.map(item => item.subject).filter(Boolean))].sort();

            subjectSelect.innerHTML = `<option value="All">All Subjects</option>`;
            subjects.forEach(subj => {
                subjectSelect.innerHTML += `<option value="${escapeHtml(subj)}">${escapeHtml(subj)}</option>`;
            });
        }

        /**
         * Updates live counters on category tabs
         */
        function updateCategoryCounts() {
            document.getElementById("count-All").textContent = allResources.length;
            document.getElementById("count-WLP").textContent = allResources.filter(r => r.category === "Weekly Lesson Plan").length;
            document.getElementById("count-BOW").textContent = allResources.filter(r => r.category === "Budget of Work").length;
            document.getElementById("count-TQ").textContent = allResources.filter(r => r.category === "Test Questionnaire").length;
            document.getElementById("count-TOS").textContent = allResources.filter(r => r.category === "Table of Specifications").length;
        }

        /**
         * Handles category pill selection
         */
        function selectCategory(category, btnElement) {
            activeCategory = category;
            document.querySelectorAll("#categoryTabs .nav-link").forEach(btn => btn.classList.remove("active"));
            btnElement.classList.add("active");
            applyFilters();
        }

        /**
         * Switches between Card Grid View and Compact Table View
         */
        function switchView(viewMode) {
            currentView = viewMode;
            const cardBtn = document.getElementById("btnCardView");
            const tableBtn = document.getElementById("btnTableView");
            const cardContainer = document.getElementById("cardViewContainer");
            const tableContainer = document.getElementById("tableViewContainer");

            if (viewMode === "card") {
                cardBtn.className = "btn btn-primary active";
                tableBtn.className = "btn btn-outline-primary bg-white";
                cardContainer.classList.remove("d-none");
                tableContainer.classList.add("d-none");
            } else {
                tableBtn.className = "btn btn-primary active";
                cardBtn.className = "btn btn-outline-primary bg-white";
                tableContainer.classList.remove("d-none");
                cardContainer.classList.add("d-none");
            }
        }

        /**
         * Resets all search and dropdown filters
         */
        function resetAllFilters() {
            document.getElementById("searchInput").value = "";
            document.getElementById("gradeFilter").value = "All";
            document.getElementById("subjectFilter").value = "All";
            document.getElementById("quarterFilter").value = "All";
            document.getElementById("sortSelect").value = "newest";
            
            const allTab = document.querySelector('#categoryTabs .nav-link[data-category="All"]');
            selectCategory("All", allTab);
        }

        /**
         * Filters, sorts, and renders the matching resources
         */
        function applyFilters() {
            const keyword = document.getElementById("searchInput").value.trim().toLowerCase();
            const grade = document.getElementById("gradeFilter").value;
            const subject = document.getElementById("subjectFilter").value;
            const quarter = document.getElementById("quarterFilter").value;
            const sortBy = document.getElementById("sortSelect").value;

            // Show or hide "Clear filters" link
            const hasActiveFilter = keyword !== "" || grade !== "All" || subject !== "All" || quarter !== "All" || activeCategory !== "All";
            document.getElementById("clearFiltersBtn").classList.toggle("d-none", !hasActiveFilter);

            // Filter
            let filtered = allResources.filter(item => {
                const matchCat = activeCategory === "All" || item.category.toLowerCase() === activeCategory.toLowerCase();
                const matchGrade = grade === "All" || item.grade.toLowerCase() === grade.toLowerCase();
                const matchSubject = subject === "All" || item.subject.toLowerCase() === subject.toLowerCase();
                const matchQuarter = quarter === "All" || item.quarter.toLowerCase() === quarter.toLowerCase();
                const matchKeyword = !keyword ||
                    item.title.toLowerCase().includes(keyword) ||
                    item.subject.toLowerCase().includes(keyword) ||
                    item.grade.toLowerCase().includes(keyword) ||
                    item.category.toLowerCase().includes(keyword);

                return matchCat && matchGrade && matchSubject && matchQuarter && matchKeyword;
            });

            // Sort
            filtered.sort((a, b) => {
                if (sortBy === "title") {
                    return a.title.localeCompare(b.title);
                } else if (sortBy === "grade") {
                    const numA = parseInt(a.grade.replace(/\D/g, "")) || 0;
                    const numB = parseInt(b.grade.replace(/\D/g, "")) || 0;
                    return numA - numB || a.title.localeCompare(b.title);
                } else {
                    // Newest First
                    return new Date(b.dateAdded) - new Date(a.dateAdded);
                }
            });

            document.getElementById("resultCountText").textContent = `Showing ${filtered.length} of ${allResources.length} resources`;
            renderCards(filtered);
            renderTable(filtered);
        }

        /**
         * Renders Responsive Card Grid
         */
        function renderCards(items) {
            const container = document.getElementById("cardViewContainer");
            container.innerHTML = "";

            if (items.length === 0) {
                container.innerHTML = `
                    <div class="col-12">
                        <div class="card border-0 shadow-sm rounded-4 text-center py-5">
                            <div class="card-body">
                                <i class="bi bi-folder-x fs-1 text-muted"></i>
                                <h6 class="fw-bold mt-2">No matching resources found</h6>
                                <p class="text-muted small mb-3">Try adjusting your search keywords or resetting the filters.</p>
                                <button class="btn btn-outline-primary btn-sm rounded-pill px-3" onclick="resetAllFilters()">Reset All Filters</button>
                            </div>
                        </div>
                    </div>`;
                return;
            }

            items.forEach((item, index) => {
                const meta = CATEGORY_META[item.category] || { badgeClass: "bg-secondary text-white", icon: "bi-file-earmark" };
                const isSampleLink = item.fileUrl.startsWith("#sample-");

                const cardCol = document.createElement("div");
                cardCol.className = "col-12 col-md-6 col-lg-4";
                cardCol.innerHTML = `
                    <div class="card resource-card h-100 shadow-sm">
                        <div class="card-body d-flex flex-column justify-content-between p-4">
                            <div>
                                <div class="d-flex justify-content-between align-items-start gap-2 mb-3">
                                    <span class="badge ${meta.badgeClass} rounded-pill px-3 py-1 fw-semibold">
                                        <i class="bi ${meta.icon} me-1"></i>${escapeHtml(item.category)}
                                    </span>
                                    <span class="badge format-badge rounded-pill px-2 py-1 fw-bold">${escapeHtml(item.fileType)}</span>
                                </div>
                                <h6 class="fw-bold text-dark mb-2 lh-base">${escapeHtml(item.title)}</h6>
                                <div class="d-flex flex-wrap gap-2 text-muted small mb-3">
                                    <span><i class="bi bi-mortarboard me-1"></i>${escapeHtml(item.grade)}</span>
                                    <span>&bull;</span>
                                    <span><i class="bi bi-book me-1"></i>${escapeHtml(item.subject)}</span>
                                    <span>&bull;</span>
                                    <span>${escapeHtml(item.quarter)}</span>
                                </div>
                            </div>

                            <div>
                                <div class="d-flex justify-content-between align-items-center border-top pt-3 mb-3 small text-muted">
                                    <span><i class="bi bi-clock me-1"></i>Added: ${escapeHtml(item.dateAdded)}</span>
                                </div>
                                <div class="d-flex gap-2">
                                    <button type="button" class="btn btn-outline-secondary btn-sm rounded-pill px-3" onclick='openPreviewModal(${JSON.stringify(item).replace(/'/g, "&#39;")})'>
                                        <i class="bi bi-eye me-1"></i>Details
                                    </button>
                                    <a href="${escapeHtml(item.fileUrl)}" ${isSampleLink ? 'onclick="handleSampleDownload(event, \'' + escapeHtml(item.title).replace(/'/g, "\\'") + '\')"' : 'target="_blank" rel="noopener noreferrer" download'} class="btn btn-primary btn-sm rounded-pill flex-grow-1 fw-semibold">
                                        <i class="bi bi-download me-1"></i>Download
                                    </a>
                                </div>
                            </div>
                        </div>
                    </div>
                `;
                container.appendChild(cardCol);
            });
        }

        /**
         * Renders Compact Table View
         */
        function renderTable(items) {
            const tbody = document.getElementById("tableBody");
            tbody.innerHTML = "";

            if (items.length === 0) {
                tbody.innerHTML = `<tr><td colspan="6" class="text-center py-5 text-muted">No matching resources found.</td></tr>`;
                return;
            }

            items.forEach(item => {
                const meta = CATEGORY_META[item.category] || { badgeClass: "bg-secondary text-white", icon: "bi-file-earmark" };
                const isSampleLink = item.fileUrl.startsWith("#sample-");

                const tr = document.createElement("tr");
                tr.innerHTML = `
                    <td class="ps-4">
                        <span class="badge ${meta.badgeClass} rounded-pill px-2 py-1">
                            <i class="bi ${meta.icon} me-1"></i>${escapeHtml(item.category)}
                        </span>
                    </td>
                    <td>
                        <div class="fw-semibold text-dark">${escapeHtml(item.title)}</div>
                        <span class="badge format-badge mt-1">${escapeHtml(item.fileType)}</span>
                    </td>
                    <td class="small">
                        <span class="fw-medium">${escapeHtml(item.grade)}</span> &bull; ${escapeHtml(item.subject)}
                    </td>
                    <td class="small">${escapeHtml(item.quarter)}</td>
                    <td class="small text-muted">${escapeHtml(item.dateAdded)}</td>
                    <td class="text-end pe-4">
                        <div class="btn-group btn-group-sm">
                            <button type="button" class="btn btn-outline-secondary" title="Preview Details" onclick='openPreviewModal(${JSON.stringify(item).replace(/'/g, "&#39;")})'>
                                <i class="bi bi-eye"></i>
                            </button>
                            <a href="${escapeHtml(item.fileUrl)}" ${isSampleLink ? 'onclick="handleSampleDownload(event, \'' + escapeHtml(item.title).replace(/'/g, "\\'") + '\')"' : 'target="_blank" rel="noopener noreferrer" download'} class="btn btn-primary fw-medium">
                                <i class="bi bi-download me-1"></i>Download
                            </a>
                        </div>
                    </td>
                `;
                tbody.appendChild(tr);
            });
        }

        /**
         * Opens the Resource Details Modal
         */
        function openPreviewModal(item) {
            const meta = CATEGORY_META[item.category] || { badgeClass: "bg-secondary text-white", icon: "bi-file-earmark" };
            const badgeEl = document.getElementById("modalCategoryBadge");
            badgeEl.className = `badge ${meta.badgeClass} rounded-pill px-3 py-1`;
            badgeEl.innerHTML = `<i class="bi ${meta.icon} me-1"></i>${escapeHtml(item.category)}`;

            document.getElementById("modalTitle").textContent = item.title;
            document.getElementById("modalGrade").textContent = item.grade;
            document.getElementById("modalSubject").textContent = item.subject;
            document.getElementById("modalQuarter").textContent = item.quarter;
            document.getElementById("modalMeta").textContent = `${item.fileType} • Added ${item.dateAdded}`;

            const dlBtn = document.getElementById("modalDownloadBtn");
            dlBtn.href = item.fileUrl;
            if (item.fileUrl.startsWith("#sample-")) {
                dlBtn.onclick = (e) => handleSampleDownload(e, item.title);
            } else {
                dlBtn.onclick = null;
            }

            const modal = new bootstrap.Modal(document.getElementById("previewModal"));
            modal.show();
        }

        /**
         * Handles clicking download on built-in sample placeholders
         */
        function handleSampleDownload(event, title) {
            event.preventDefault();
            showToast(`Sample Preview: "${title}". Connect your Google Sheet or GitHub /files folder to enable real file downloads.`, "info");
        }

        /**
         * Live Tester: Tests user's pasted Google Sheet CSV URL
         */
        function testLiveGoogleSheet() {
            const urlInput = document.getElementById("testCsvUrlInput").value.trim();
            const feedback = document.getElementById("testerFeedback");

            if (!urlInput) {
                feedback.innerHTML = `<span class="text-danger">Please paste your published Google Sheet CSV URL first.</span>`;
                return;
            }
            loadFromGoogleSheet(urlInput, true);
        }

        /**
         * Clears tested URL from localStorage and reverts to built-in sample data
         */
        function revertToSampleData() {
            localStorage.removeItem("teacher_portal_test_csv_url");
            document.getElementById("testCsvUrlInput").value = "";
            document.getElementById("testerFeedback").innerHTML = `<span class="text-muted">Reverted to built-in sample data.</span>`;
            updateConfigSnippet("PASTE_YOUR_GOOGLE_SHEET_CSV_URL_HERE");
            loadBuiltInSampleData();
            showToast("Portal reset to built-in sample data.", "secondary");
        }

        /**
         * Updates the copyable config snippet inside the Setup Modal
         */
        function updateConfigSnippet(url) {
            document.getElementById("configCodeBlock").textContent = `const SHEET_CSV_URL = "${url}";`;
        }

        /**
         * Copies the SHEET_CSV_URL snippet to clipboard
         */
        function copyConfigSnippet() {
            const text = document.getElementById("configCodeBlock").textContent;
            const textarea = document.createElement("textarea");
            textarea.value = text;
            document.body.appendChild(textarea);
            textarea.select();
            document.execCommand("copy");
            document.body.removeChild(textarea);
            showToast("Configuration line copied to clipboard!", "success");
        }

        /**
         * Generates and downloads the CSV template for Google Sheets
         */
        function downloadSampleCSV() {
            const csvRows = [
                ["title", "category", "grade", "subject", "quarter", "fileType", "fileUrl", "dateAdded"],
                ["Q1 Week 1: Quadratic Equations DLL", "Weekly Lesson Plan", "Grade 9", "Mathematics", "Quarter 1", "DOCX", "https://drive.google.com/your-file-link", "2026-08-10"],
                ["Grade 8 Science Full-Year BOW", "Budget of Work", "Grade 8", "Science", "Whole Year", "XLSX", "https://drive.google.com/your-file-link", "2026-08-05"],
                ["First Periodical Exam - English 10", "Test Questionnaire", "Grade 10", "English", "Quarter 1", "PDF", "https://drive.google.com/your-file-link", "2026-10-12"],
                ["Q1 Periodical Exam TOS", "Table of Specifications", "Grade 10", "English", "Quarter 1", "XLSX", "https://drive.google.com/your-file-link", "2026-10-12"]
            ];

            const csvContent = csvRows.map(row => row.map(val => `"${val.replace(/"/g, '""')}"`).join(",")).join("\n");
            const blob = new Blob([csvContent], { type: "text/csv;charset=utf-8;" });
            const url = URL.createObjectURL(blob);

            const link = document.createElement("a");
            link.href = url;
            link.setAttribute("download", "teacher_portal_template.csv");
            document.body.appendChild(link);
            link.click();
            document.body.removeChild(link);
            URL.revokeObjectURL(url);
        }

        /**
         * Non-blocking Bootstrap Toast Notification
         */
        function showToast(message, type = "primary") {
            const container = document.getElementById("toastContainer");
            const toastId = "toast-" + Date.now();
            const bgMap = {
                success: "text-bg-success",
                warning: "text-bg-warning",
                info: "text-bg-primary",
                secondary: "text-bg-secondary",
                primary: "text-bg-primary"
            };

            const toastHtml = `
                <div id="${toastId}" class="toast align-items-center ${bgMap[type] || "text-bg-primary"} border-0 shadow mb-2 show" role="alert">
                    <div class="d-flex">
                        <div class="toast-body small fw-medium">${escapeHtml(message)}</div>
                        <button type="button" class="btn-close btn-close-white me-2 m-auto" onclick="document.getElementById('${toastId}').remove()"></button>
                    </div>
                </div>
            `;
            container.insertAdjacentHTML("beforeend", toastHtml);
            setTimeout(() => {
                const el = document.getElementById(toastId);
                if (el) el.remove();
            }, 4500);
        }

        /**
         * Escapes HTML characters to prevent XSS from external CSV inputs
         */
        function escapeHtml(str) {
            if (str === null || str === undefined) return "";
            return String(str)
                .replace(/&/g, "&amp;")
                .replace(/</g, "&lt;")
                .replace(/>/g, "&gt;")
                .replace(/"/g, "&quot;")
                .replace(/'/g, "&#039;");
        }
    </script>
</body>
</html>
