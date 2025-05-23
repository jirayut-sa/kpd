<!DOCTYPE html>
<html lang="th">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>ระบบเช็คชื่อนักเรียนออนไลน์ โรงเรียนบ้านโคกประดู่</title>
    <link href="https://cdn.jsdelivr.net/npm/tailwindcss@2.2.19/dist/tailwind.min.css" rel="stylesheet">
    <link href="https://cdn.jsdelivr.net/npm/sweetalert2@11.0.19/dist/sweetalert2.min.css" rel="stylesheet">
    <link href="https://cdn.jsdelivr.net/npm/select2@4.1.0-rc.0/dist/css/select2.min.css" rel="stylesheet" />
    <link href="https://fonts.googleapis.com/css2?family=Sarabun:wght@400;700&display=swap" rel="stylesheet">
    <script src="https://cdn.jsdelivr.net/npm/jquery@3.6.0/dist/jquery.min.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/jquery-validation@1.19.3/dist/jquery.validate.min.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/sweetalert2@11.0.19/dist/sweetalert2.all.min.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/select2@4.1.0-rc.0/dist/js/select2.min.js"></script>
    <script src="https://cdn.sheetjs.com/xlsx-latest/package/dist/xlsx.full.min.js"></script>
    <style>
        :root {
            --primary-color: #FF69B4; /* Pink */
            --secondary-color: #4CAF50; /* Green */
            --blue-color: #2196F3;
            --red-color: #F44336;
            --yellow-color: #FFC107;
            --orange-color: #FF9800;
            --gray-color: #9E9E9E;
        }
        
        body {
            font-family: 'Sarabun', sans-serif;
            background-color: #f8f9fa;
        }
        
        .tab-content {
            display: none;
        }
        
        .tab-content.active {
            display: block;
        }
        
        .nav-tab {
            cursor: pointer;
            transition: all 0.3s;
            border-bottom: 3px solid transparent;
        }
        
        .nav-tab.active {
            border-bottom: 3px solid var(--primary-color);
            background-color: var(--primary-color);
            color: white;
            font-weight: bold;
            border-radius: 12px;
            padding: 8px 16px;
        }
        
        .nav-tab:hover:not(.active) {
            border-bottom: 3px solid var(--secondary-color);
            color: var(--secondary-color);
        }
        
        .stat-card {
            border-radius: 10px;
            box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
            transition: transform 0.3s;
        }
        
        .stat-card:hover {
            transform: translateY(-5px);
        }
        
        .btn-primary {
            background-color: var(--primary-color);
            color: white;
        }
        
        .btn-primary:hover {
            background-color: #e05a9b;
        }
        
        .btn-secondary {
            background-color: var(--secondary-color);
            color: white;
        }
        
        .btn-secondary:hover {
            background-color: #3d8b40;
        }
        
        .btn-danger {
            background-color: var(--red-color);
            color: white;
        }
        
        .btn-danger:hover {
            background-color: #d32f2f;
        }
        
        .modal {
            display: none;
            position: fixed;
            z-index: 1000;
            left: 0;
            top: 0;
            width: 100%;
            height: 100%;
            overflow: auto;
            background-color: rgba(0, 0, 0, 0.4);
        }
        
        .modal-content {
            background-color: #fefefe;
            margin: 10% auto;
            padding: 20px;
            border: 1px solid #888;
            width: 80%;
            max-width: 600px;
            border-radius: 10px;
            box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
        }
        
        .close {
            color: #aaa;
            float: right;
            font-size: 28px;
            font-weight: bold;
            cursor: pointer;
        }
        
        .close:hover {
            color: black;
        }
        
        table {
            width: 100%;
            border-collapse: collapse;
        }
        
        th, td {
            padding: 12px;
            text-align: left;
            border-bottom: 1px solid #ddd;
        }
        
        th {
            background-color: #f2f2f2;
        }
        
        tr:hover {
            background-color: #f5f5f5;
        }
        
        .attendance-present {
            background-color: var(--secondary-color);
            color: white;
        }
        
        .attendance-absent {
            background-color: var(--red-color);
            color: white;
        }
        
        .attendance-leave {
            background-color: var(--yellow-color);
            color: black;
        }
        
        .attendance-late {
            background-color: var(--primary-color);
            color: white;
        }
        
        .attendance-none {
            background-color: var(--gray-color);
            color: white;
        }
        
        .mark-late {
            background-color: var(--primary-color) !important;
            color: white;
        }
        
        .mark-late:hover {
            background-color: #e05a9b !important;
        }
        
        .loading {
            display: none;
            position: fixed;
            z-index: 1100;
            left: 0;
            top: 0;
            width: 100%;
            height: 100%;
            background-color: rgba(255, 255, 255, 0.7);
        }
        
        .loading-content {
            position: absolute;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            text-align: center;
        }
        
        .spinner {
            border: 5px solid #f3f3f3;
            border-top: 5px solid var(--primary-color);
            border-radius: 50%;
            width: 40px;
            height: 40px;
            animation: spin 1s linear infinite;
            margin: 0 auto;
        }
        
        @keyframes spin {
            0% { transform: rotate(0deg); }
            100% { transform: rotate(360deg); }
        }
        
        .error {
            color: var(--red-color);
            font-size: 0.8rem;
            margin-top: 5px;
        }
        
        .select2-container--default .select2-selection--multiple {
            border: 1px solid #d1d5db;
            border-radius: 0.5rem;
            padding: 0.5rem;
        }
        
        .select2-container--default .select2-selection--multiple .select2-selection__choice {
            background-color: #FF69B4;
            color: white;
            border: none;
        }
        
        .select2-container--default .select2-selection--multiple .select2-selection__choice__remove {
            color: white;
        }
    </style>
</head>
<body>
    <!-- Header -->
    <header class="bg-white py-4">
        <div class="container mx-auto px-4 flex flex-col items-center">
            <img src="https://drive.google.com/file/d/1vzZx4sJ0WkUwoOnxCCx6oBB6ne_XsFa-/view?usp=sharing" alt="โลโก้โรงเรียนบ้านโคกประดู่" class="h-32 max-w-full">
            <h1 class="text-2xl font-bold mt-2 text-center">ระบบเช็คชื่อนักเรียนออนไลน์ โรงเรียนบ้านโคกประดู่</h1>
        </div>
    </header>

    <!-- Navigation Tabs -->
    <nav class="bg-white shadow-md">
        <div class="container mx-auto px-4">
            <div class="flex flex-wrap justify-center md:justify-start space-x-1 md:space-x-6 py-4">
                <div class="nav-tab active px-3 py-2" data-tab="dashboard">แดชบอร์ด</div>
                <div class="nav-tab px-3 py-2" data-tab="subjects">จัดการรายวิชา</div>
                <div class="nav-tab px-3 py-2" data-tab="students">จัดการนักเรียน</div>
                <div class="nav-tab px-3 py-2" data-tab="attendance">จัดการเช็คเวลาเรียน</div>
                <div class="nav-tab px-3 py-2" data-tab="reports">รายงาน</div>
            </div>
        </div>
    </nav>

    <!-- Main Content -->
    <main class="container mx-auto px-4 py-6">
        <!-- Dashboard Tab -->
        <div id="dashboard" class="tab-content active">
            <h2 class="text-2xl font-bold mb-6">แดชบอร์ด</h2>
            
            <!-- Stats Cards -->
            <div class="grid grid-cols-1 md:grid-cols-3 lg:grid-cols-5 gap-4 mb-8">
                <div class="stat-card bg-blue-500 text-white p-4">
                    <h3 class="text-lg font-semibold">นักเรียนทั้งหมด</h3>
                    <p class="text-3xl font-bold" id="total-students">0</p>
                </div>
                <div class="stat-card bg-green-500 text-white p-4">
                    <h3 class="text-lg font-semibold">มาเรียนวันนี้</h3>
                    <p class="text-3xl font-bold" id="present-students">0</p>
                </div>
                <div class="stat-card bg-red-500 text-white p-4">
                    <h3 class="text-lg font-semibold">ขาดเรียนวันนี้</h3>
                    <p class="text-3xl font-bold" id="absent-students">0</p>
                </div>
                <div class="stat-card bg-yellow-500 text-white p-4">
                    <h3 class="text-lg font-semibold">ลาวันนี้</h3>
                    <p class="text-3xl font-bold" id="leave-students">0</p>
                </div>
                <div class="stat-card bg-pink-500 text-white p-4">
                    <h3 class="text-lg font-semibold">มาสายวันนี้</h3>
                    <p class="text-3xl font-bold" id="late-students">0</p>
                </div>
            </div>
            
            <!-- Top Students Tables -->
            <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
                <div class="bg-white p-4 rounded-lg shadow">
                    <h3 class="text-xl font-bold mb-4 text-green-600">นักเรียนที่มาเรียนมากที่สุด 5 อันดับแรก (รวมทั้งหมด)</h3>
                    <table>
                        <thead>
                            <tr>
                                <th>ลำดับ</th>
                                <th>ชื่อ-นามสกุล</th>
                                <th>ระดับชั้น</th>
                                <th>ห้อง</th>
                                <th>จำนวนวัน</th>
                            </tr>
                        </thead>
                        <tbody id="top-present"></tbody>
                    </table>
                </div>
                
                <div class="bg-white p-4 rounded-lg shadow">
                    <h3 class="text-xl font-bold mb-4 text-red-600">นักเรียนที่ขาดเรียนมากที่สุด 5 อันดับแรก (รวมทั้งหมด)</h3>
                    <table>
                        <thead>
                            <tr>
                                <th>ลำดับ</th>
                                <th>ชื่อ-นามสกุล</th>
                                <th>ระดับชั้น</th>
                                <th>ห้อง</th>
                                <th>จำนวนวัน</th>
                            </tr>
                        </thead>
                        <tbody id="top-absent"></tbody>
                    </table>
                </div>
            </div>
        </div>

        <!-- Subjects Tab -->
        <div id="subjects" class="tab-content">
            <div class="flex justify-between items-center mb-6">
                <h2 class="text-2xl font-bold">จัดการรายวิชา</h2>
                <div class="space-x-2">
                    <button id="download-subjects-template" class="bg-blue-500 hover:bg-blue-600 text-white px-4 py-2 rounded-lg">
                        ดาวน์โหลดเทมเพลต (.CSV)
                    </button>
                    <button id="import-subjects-btn" class="bg-green-500 hover:bg-green-600 text-white px-4 py-2 rounded-lg">
                        นำเข้ารายวิชา
                    </button>
                    <button id="add-subject-btn" class="bg-pink-500 hover:bg-pink-600 text-white px-4 py-2 rounded-lg">
                        เพิ่มรายวิชา
                    </button>
                </div>
            </div>
            
            <div class="bg-white rounded-lg shadow p-4">
                <table id="subjects-table">
                    <thead>
                        <tr>
                            <th>รหัสวิชา</th>
                            <th>ชื่อรายวิชา</th>
                            <th>จัดการ</th>
                        </tr>
                    </thead>
                    <tbody></tbody>
                </table>
            </div>
        </div>

        <!-- Students Tab -->
        <div id="students" class="tab-content">
            <div class="flex justify-between items-center mb-6">
                <h2 class="text-2xl font-bold">จัดการนักเรียน</h2>
                <div class="space-x-2">
                    <button id="download-students-template" class="bg-blue-500 hover:bg-blue-600 text-white px-4 py-2 rounded-lg">
                        ดาวน์โหลดเทมเพลต (.CSV)
                    </button>
                    <button id="import-students-btn" class="bg-green-500 hover:bg-green-600 text-white px-4 py-2 rounded-lg">
                        นำเข้านักเรียน
                    </button>
                    <button id="add-student-btn" class="bg-pink-500 hover:bg-pink-600 text-white px-4 py-2 rounded-lg">
                        เพิ่มนักเรียน
                    </button>
                </div>
            </div>
            
            <div id="students-table-container" class="bg-white rounded-lg shadow p-4">
                <table id="students-table">
                    <thead>
                        <tr>
                            <th>รหัสนักเรียน</th>
                            <th>ชื่อ-นามสกุล</th>
                            <th>ระดับชั้น</th>
                            <th>ห้อง</th>
                            <th>รายวิชา</th>
                            <th>จัดการ</th>
                        </tr>
                    </thead>
                    <tbody></tbody>
                </table>
            </div>
        </div>

        <!-- Attendance Tab -->
        <div id="attendance" class="tab-content">
            <h2 class="text-2xl font-bold mb-6">จัดการเช็คเวลาเรียน</h2>
            
            <div class="bg-white rounded-lg shadow p-6 mb-6">
                <div class="grid grid-cols-1 md:grid-cols-4 gap-4 mb-4">
                    <div>
                        <label class="block text-gray-700 mb-2">วันที่</label>
                        <input type="date" id="attendance-date" class="w-full px-3 py-2 border rounded-lg">
                    </div>
                    <div>
                        <label class="block text-gray-700 mb-2">รายวิชา</label>
                        <select id="attendance-subject" class="w-full px-3 py-2 border rounded-lg">
                            <option value="all">ทุกรายวิชา</option>
                        </select>
                    </div>
                    <div>
                        <label class="block text-gray-700 mb-2">ระดับชั้น</label>
                        <select id="attendance-class" class="w-full px-3 py-2 border rounded-lg">
                            <option value="all">ทุกระดับชั้น</option>
                            <option value="อ.1">อนุบาล 1</option>
                            <option value="อ.2">อนุบาล 2</option>
                            <option value="อ.3">อนุบาล 3</option>
                            <option value="ป.1">ประถมศึกษาปีที่ 1</option>
                            <option value="ป.2">ประถมศึกษาปีที่ 2</option>
                            <option value="ป.3">ประถมศึกษาปีที่ 3</option>
                            <option value="ป.4">ประถมศึกษาปีที่ 4</option>
                            <option value="ป.5">ประถมศึกษาปีที่ 5</option>
                            <option value="ป.6">ประถมศึกษาปีที่ 6</option>
                            <option value="ม.1">มัธยมศึกษาปีที่ 1</option>
                            <option value="ม.2">มัธยมศึกษาปีที่ 2</option>
                            <option value="ม.3">มัธยมศึกษาปีที่ 3</option>
                            <option value="ม.4">มัธยมศึกษาปีที่ 4</option>
                            <option value="ม.5">มัธยมศึกษาปีที่ 5</option>
                            <option value="ม.6">มัธยมศึกษาปีที่ 6</option>
                        </select>
                    </div>
                    <div>
                        <label class="block text-gray-700 mb-2">ห้องเรียน</label>
                        <select id="attendance-classroom" class="w-full px-3 py-2 border rounded-lg">
                            <option value="">ทุกห้องเรียน</option>
                        </select>
                    </div>
                </div>
                <div class="text-center">
                    <button id="start-attendance" class="bg-pink-500 hover:bg-pink-600 text-white px-6 py-2 rounded-lg">
                        เริ่มเช็คเวลาเรียน
                    </button>
                </div>
            </div>
            
            <div id="attendance-list" class="hidden">
                <div class="flex justify-between items-center mb-4">
                    <h3 class="text-xl font-bold">รายชื่อนักเรียน</h3>
                    <div>
                        <button id="mark-all-present" class="bg-green-500 hover:bg-green-600 text-white px-4 py-2 rounded-lg mr-2">
                            มาเรียนทั้งหมด
                        </button>
                        <button id="reset-attendance" class="bg-gray-500 hover:bg-gray-600 text-white px-4 py-2 rounded-lg">
                            ยกเลิกทั้งหมด
                        </button>
                    </div>
                </div>
                
                <div class="bg-white rounded-lg shadow p-4 mb-4">
                    <table id="attendance-table">
                        <thead>
                            <tr>
                                <th>รหัสนักเรียน</th>
                                <th>ชื่อ-นามสกุล</th>
                                <th>ระดับชั้น</th>
                                <th>ห้อง</th>
                                <th>รายวิชา</th>
                                <th>สถานะ</th>
                                <th>จัดการ</th>
                            </tr>
                        </thead>
                        <tbody></tbody>
                    </table>
                </div>
                
                <div class="text-center">
                    <button id="save-attendance" class="bg-pink-500 hover:bg-pink-600 text-white px-6 py-2 rounded-lg">
                        บันทึกเวลาเรียน
                    </button>
                </div>
            </div>
        </div>

        <!-- Reports Tab -->
        <div id="reports" class="tab-content">
            <h2 class="text-2xl font-bold mb-6">รายงาน</h2>
            
            <div class="bg-white rounded-lg shadow p-6 mb-6">
                <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-5 gap-4 mb-4">
                    <div>
                        <label class="block text-gray-700 mb-2">วันที่เริ่มต้น</label>
                        <input type="date" id="report-start-date" class="w-full px-3 py-2 border rounded-lg">
                    </div>
                    <div>
                        <label class="block text-gray-700 mb-2">วันที่สิ้นสุด</label>
                        <input type="date" id="report-end-date" class="w-full px-3 py-2 border rounded-lg">
                    </div>
                    <div>
                        <label class="block text-gray-700 mb-2">ระดับชั้น</label>
                        <select id="report-class" class="w-full px-3 py-2 border rounded-lg">
                            <option value="">ทุกระดับชั้น</option>
                            <option value="อ.1">อนุบาล 1</option>
                            <option value="อ.2">อนุบาล 2</option>
                            <option value="อ.3">อนุบาล 3</option>
                            <option value="ป.1">ประถมศึกษาปีที่ 1</option>
                            <option value="ป.2">ประถมศึกษาปีที่ 2</option>
                            <option value="ป.3">ประถมศึกษาปีที่ 3</option>
                            <option value="ป.4">ประถมศึกษาปีที่ 4</option>
                            <option value="ป.5">ประถมศึกษาปีที่ 5</option>
                            <option value="ป.6">ประถมศึกษาปีที่ 6</option>
                            <option value="ม.1">มัธยมศึกษาปีที่ 1</option>
                            <option value="ม.2">มัธยมศึกษาปีที่ 2</option>
                            <option value="ม.3">มัธยมศึกษาปีที่ 3</option>
                            <option value="ม.4">มัธยมศึกษาปีที่ 4</option>
                            <option value="ม.5">มัธยมศึกษาปีที่ 5</option>
                            <option value="ม.6">มัธยมศึกษาปีที่ 6</option>
                        </select>
                    </div>
                    <div>
                        <label class="block text-gray-700 mb-2">ห้องเรียน</label>
                        <select id="report-classroom" class="w-full px-3 py-2 border rounded-lg">
                            <option value="">ทุกห้องเรียน</option>
                        </select>
                    </div>
                    <div>
                        <label class="block text-gray-700 mb-2">นักเรียน</label>
                        <select id="report-student" class="w-full px-3 py-2 border rounded-lg">
                            <option value="">ทุกคน</option>
                        </select>
                    </div>
                </div>
                <div class="text-center">
                    <button id="generate-report" class="bg-pink-500 hover:bg-pink-600 text-white px-6 py-2 rounded-lg">
                        ดูรายงาน
                    </button>
                </div>
            </div>
            
            <div id="report-results" class="hidden">
                <div class="grid grid-cols-1 md:grid-cols-3 lg:grid-cols-5 gap-4 mb-6">
                    <div class="stat-card bg-blue-500 text-white p-4">
                        <h3 class="text-lg font-semibold">นักเรียนทั้งหมด</h3>
                        <p class="text-3xl font-bold" id="report-total-students">0</p>
                    </div>
                    <div class="stat-card bg-green-500 text-white p-4">
                        <h3 class="text-lg font-semibold">มาเรียน</h3>
                        <p class="text-3xl font-bold" id="report-present-students">0</p>
                    </div>
                    <div class="stat-card bg-red-500 text-white p-4">
                        <h3 class="text-lg font-semibold">ขาดเรียน</h3>
                        <p class="text-3xl font-bold" id="report-absent-students">0</p>
                    </div>
                    <div class="stat-card bg-yellow-500 text-white p-4">
                        <h3 class="text-lg font-semibold">ลา</h3>
                        <p class="text-3xl font-bold" id="report-leave-students">0</p>
                    </div>
                    <div class="stat-card bg-pink-500 text-white p-4">
                        <h3 class="text-lg font-semibold">มาสาย</h3>
                        <p class="text-3xl font-bold" id="report-late-students">0</p>
                    </div>
                </div>

                <!-- Export Buttons -->
                <div class="text-center mb-4 space-x-2">
                    <button id="export-csv" class="bg-blue-500 hover:bg-blue-600 text-white px-6 py-2 rounded-lg">
                        ดาวน์โหลดรายงาน (.CSV)
                    </button>
                    <button id="export-xlsx" class="bg-green-500 hover:bg-green-600 text-white px-6 py-2 rounded-lg">
                        ดาวน์โหลดรายงาน (.XLSX)
                    </button>
                </div>

                <div class="bg-white rounded-lg shadow p-4">
                    <table id="report-table">
                        <thead>
                            <tr>
                                <th>วันที่</th>
                                <th>รหัสนักเรียน</th>
                                <th>ชื่อ-นามสกุล</th>
                                <th>ระดับชั้น</th>
                                <th>ห้อง</th>
                                <th>รายวิชา</th>
                                <th>สถานะ</th>
                            </tr>
                        </thead>
                        <tbody></tbody>
                    </table>
                </div>
            </div>
        </div>
    </main>

    <!-- Footer -->
    <footer class="bg-white py-4 mt-8 border-t">
        <div class="container mx-auto px-4 text-center text-gray-600">
            © 2025 ระบบเช็คชื่อนักเรียนออนไลน์ โรงเรียนบ้านโคกประดู่ | นายจิรายุทธ แสงสิน
        </div>
    </footer>

    <!-- Subject Modal -->
    <div id="subject-modal" class="modal">
        <div class="modal-content">
            <span class="close">×</span>
            <h2 class="text-xl font-bold mb-4" id="subject-modal-title">เพิ่มรายวิชา</h2>
            <form id="subject-form">
                <input type="hidden" id="subject-id">
                <div class="mb-4">
                    <label class="block text-gray-700 mb-2">รหัสวิชา</label>
                    <input type="text" id="subject-code" name="subject-code" class="w-full px-3 py-2 border rounded-lg" required>
                </div>
                <div class="mb-4">
                    <label class="block text-gray-700 mb-2">ชื่อรายวิชา</label>
                    <input type="text" id="subject-name" name="subject-name" class="w-full px-3 py-2 border rounded-lg" required>
                </div>
                <div class="text-right">
                    <button type="button" class="close-modal bg-gray-500 hover:bg-gray-600 text-white px-4 py-2 rounded-lg mr-2">
                        ยกเลิก
                    </button>
                    <button type="submit" class="bg-pink-500 hover:bg-pink-600 text-white px-4 py-2 rounded-lg">
                        บันทึก
                    </button>
                </div>
            </form>
        </div>
    </div>

    <!-- Student Modal -->
    <div id="student-modal" class="modal">
        <div class="modal-content">
            <span class="close">×</span>
            <h2 class="text-xl font-bold mb-4" id="student-modal-title">เพิ่มนักเรียน</h2>
            <form id="student-form">
                <input type="hidden" id="student-id">
                <div class="mb-4">
                    <label class="block text-gray-700 mb-2">รหัสนักเรียน</label>
                    <input type="text" id="student-code" name="student-code" class="w-full px-3 py-2 border rounded-lg" required>
                </div>
                <div class="mb-4">
                    <label class="block text-gray-700 mb-2">ชื่อ-นามสกุล</label>
                    <input type="text" id="student-name" name="student-name" class="w-full px-3 py-2 border rounded-lg" required>
                </div>
                <div class="mb-4">
                    <label class="block text-gray-700 mb-2">ระดับชั้น</label>
                    <select id="student-class" name="student-class" class="w-full px-3 py-2 border rounded-lg" required>
                        <option value="">เลือกระดับชั้น</option>
                        <option value="อ.1">อนุบาล 1</option>
                        <option value="อ.2">อนุบาล 2</option>
                        <option value="อ.3">อนุบาล 3</option>
                        <option value="ป.1">ประถมศึกษาปีที่ 1</option>
                        <option value="ป.2">ประถมศึกษาปีที่ 2</option>
                        <option value="ป.3">ประถมศึกษาปีที่ 3</option>
                        <option value="ป.4">ประถมศึกษาปีที่ 4</option>
                        <option value="ป.5">ประถมศึกษาปีที่ 5</option>
                        <option value="ป.6">ประถมศึกษาปีที่ 6</option>
                        <option value="ม.1">มัธยมศึกษาปีที่ 1</option>
                        <option value="ม.2">มัธยมศึกษาปีที่ 2</option>
                        <option value="ม.3">มัธยมศึกษาปีที่ 3</option>
                        <option value="ม.4">มัธยมศึกษาปีที่ 4</option>
                        <option value="ม.5">มัธยมศึกษาปีที่ 5</option>
                        <option value="ม.6">มัธยมศึกษาปีที่ 6</option>
                    </select>
                </div>
                <div class="mb-4">
                    <label class="block text-gray-700 mb-2">ห้องเรียน</label>
                    <input type="text" id="student-classroom" name="student-classroom" class="w-full px-3 py-2 border rounded-lg" placeholder="เช่น 3/1, A, ห้อง 1" required>
                </div>
                <div class="mb-4">
                    <label class="block text-gray-700 mb-2">รายวิชา</label>
                    <select id="student-subject" name="student-subject" class="w-full px-3 py-2 border rounded-lg" multiple required>
                        <option value="">เลือกรายวิชา</option>
                    </select>
                </div>
                <div class="text-right">
                    <button type="button" class="close-modal bg-gray-500 hover:bg-gray-600 text-white px-4 py-2 rounded-lg mr-2">
                        ยกเลิก
                    </button>
                    <button type="submit" class="bg-pink-500 hover:bg-pink-600 text-white px-4 py-2 rounded-lg">
                        บันทึก
                    </button>
                </div>
            </form>
        </div>
    </div>

    <!-- Import Subjects Modal -->
    <div id="import-subjects-modal" class="modal">
        <div class="modal-content">
            <span class="close">×</span>
            <h2 class="text-xl font-bold mb-4">นำเข้ารายวิชาจากไฟล์ CSV</h2>
            <form id="import-subjects-form">
                <div class="mb-4">
                    <label class="block text-gray-700 mb-2">เลือกไฟล์ CSV</label>
                    <input type="file" id="subjects-csv-file" name="csv-file" accept=".csv" class="w-full px-3 py-2 border rounded-lg" required>
                    <p class="text-sm text-gray-600 mt-2">กรุณาใช้เทมเพลตที่ดาวน์โหลดจากระบบ</p>
                </div>
                <div class="text-right">
                    <button type="button" class="close-modal bg-gray-500 hover:bg-gray-600 text-white px-4 py-2 rounded-lg mr-2">
                        ยกเลิก
                    </button>
                    <button type="submit" class="bg-green-500 hover:bg-green-600 text-white px-4 py-2 rounded-lg">
                        นำเข้า
                    </button>
                </div>
            </form>
        </div>
    </div>

    <!-- Import Students Modal -->
    <div id="import-students-modal" class="modal">
        <div class="modal-content">
            <span class="close">×</span>
            <h2 class="text-xl font-bold mb-4">นำเข้านักเรียนจากไฟล์ CSV</h2>
            <form id="import-students-form">
                <div class="mb-4">
                    <label class="block text-gray-700 mb-2">เลือกไฟล์ CSV</label>
                    <input type="file" id="students-csv-file" name="csv-file" accept=".csv" class="w-full px-3 py-2 border rounded-lg" required>
                    <p class="text-sm text-gray-600 mt-2">กรุณาใช้เทมเพลตที่ดาวน์โหลดจากระบบ</p>
                </div>
                <div class="text-right">
                    <button type="button" class="close-modal bg-gray-500 hover:bg-gray-600 text-white px-4 py-2 rounded-lg mr-2">
                        ยกเลิก
                    </button>
                    <button type="submit" class="bg-green-500 hover:bg-green-600 text-white px-4 py-2 rounded-lg">
                        นำเข้า
                    </button>
                </div>
            </form>
        </div>
    </div>

    <!-- Loading Overlay -->
    <div class="loading" id="loading-overlay">
        <div class="loading-content">
            <div class="spinner"></div>
            <p class="mt-2 text-pink-600 font-semibold">กำลังประมวลผล...</p>
        </div>
    </div>

    <script>
        // Global variable to store current attendance data
        let currentAttendance = [];

        // Utility to normalize date to YYYY-MM-DD
        function normalizeDate(dateStr) {
            try {
                const date = new Date(dateStr);
                if (isNaN(date.getTime())) {
                    console.warn(`Invalid date: ${dateStr}`);
                    return '';
                }
                return date.toISOString().split('T')[0];
            } catch (e) {
                console.warn(`Error normalizing date ${dateStr}:`, e);
                return '';
            }
        }

        function getShortClassName(fullName) {
            const map = {
                'อนุบาล 1': 'อ.1',
                'อนุบาล 2': 'อ.2',
                'อนุบาล 3': 'อ.3',
                'ประถมศึกษาปีที่ 1': 'ป.1',
                'ประถมศึกษาปีที่ 2': 'ป.2',
                'ประถมศึกษาปีที่ 3': 'ป.3',
                'ประถมศึกษาปีที่ 4': 'ป.4',
                'ประถมศึกษาปีที่ 5': 'ป.5',
                'ประถมศึกษาปีที่ 6': 'ป.6',
                'มัธยมศึกษาปีที่ 1': 'ม.1',
                'มัธยมศึกษาปีที่ 2': 'ม.2',
                'มัธยมศึกษาปีที่ 3': 'ม.3',
                'มัธยมศึกษาปีที่ 4': 'ม.4',
                'มัธยมศึกษาปีที่ 5': 'ม.5',
                'มัธยมศึกษาปีที่ 6': 'ม.6'
            };
            return map[fullName] || fullName;
        }

        // Error handling
        function handleError(error) {
            hideLoading();
            console.error('Error:', error);
            Swal.fire({
                icon: 'error',
                title: 'เกิดข้อผิดพลาด',
                text: error.message || 'ไม่สามารถดำเนินการได้ กรุณาลองใหม่',
                confirmButtonColor: '#FF69B4'
            });
        }

        // Tab Navigation
        function initializeTabs() {
            const tabs = document.querySelectorAll('.nav-tab');
            const contents = document.querySelectorAll('.tab-content');

            tabs.forEach(tab => {
                tab.addEventListener('click', function() {
                    tabs.forEach(t => t.classList.remove('active'));
                    contents.forEach(c => c.classList.remove('active'));

                    this.classList.add('active');
                    const tabId = this.dataset.tab;
                    const content = document.getElementById(tabId);
                    if (content) {
                        content.classList.add('active');
                        if (tabId === 'students') {
                            loadStudents();
                        } else if (tabId === 'dashboard') {
                            loadDashboard();
                        } else if (tabId === 'subjects') {
                            loadSubjects();
                        } else if (tabId === 'reports') {
                            loadStudents();
                        }
                    } else {
                        console.error(`Content for tab ${tabId} not found`);
                    }
                });
            });
        }

        // Modal Functions
        function openModal(modalId) {
            const modal = document.getElementById(modalId);
            if (modal) {
                modal.style.display = 'block';
            } else {
                console.error(`Modal ${modalId} not found`);
            }
        }
        
        function closeModal(modalId) {
            const modal = document.getElementById(modalId);
            if (modal) {
                modal.style.display = 'none';
            } else {
                console.error(`Modal ${modalId} not found`);
            }
        }
        
        document.querySelectorAll('.close, .close-modal').forEach(element => {
            element.addEventListener('click', function() {
                const modal = this.closest('.modal');
                if (modal) closeModal(modal.id);
            });
        });
        
        window.addEventListener('click', function(event) {
            if (event.target.classList.contains('modal')) {
                closeModal(event.target.id);
            }
        });

        // Loading Functions
        function showLoading() {
            const overlay = document.getElementById('loading-overlay');
            if (overlay) {
                overlay.style.display = 'block';
            }
        }
        
        function hideLoading() {
            const overlay = document.getElementById('loading-overlay');
            if (overlay) {
                overlay.style.display = 'none';
            }
        }

        // Dashboard Stats
        function loadDashboard() {
            console.log('Loading dashboard...');
            showLoading();
            google.script.run
                .withSuccessHandler(function(students) {
                    if (!Array.isArray(students)) {
                        hideLoading();
                        Swal.fire({
                            icon: 'warning',
                            title: 'ไม่พบข้อมูลนักเรียน',
                            text: 'กรุณาเพิ่มข้อมูลนักเรียนก่อน',
                            confirmButtonColor: '#FF69B4'
                        });
                        return;
                    }
                    google.script.run
                        .withSuccessHandler(function(attendance) {
                            const totalStudents = students.length || 0;
                            const validAttendance = Array.isArray(attendance) ? attendance : [];
                            const today = normalizeDate(new Date());

                            const todayAttendance = validAttendance.filter(a => normalizeDate(a.date) === today);

                            const presentCount = todayAttendance.filter(a => a.status === 'present').length;
                            const absentCount = todayAttendance.filter(a => a.status === 'absent').length;
                            const leaveCount = todayAttendance.filter(a => a.status === 'leave').length;
                            const lateCount = todayAttendance.filter(a => a.status === 'late').length;

                            document.getElementById('total-students').textContent = totalStudents;
                            document.getElementById('present-students').textContent = presentCount;
                            document.getElementById('absent-students').textContent = absentCount;
                            document.getElementById('leave-students').textContent = leaveCount;
                            document.getElementById('late-students').textContent = lateCount;

                            const presentByStudent = {};
                            validAttendance.forEach(a => {
                                if (a.status === 'present') {
                                    presentByStudent[a.studentId] = (presentByStudent[a.studentId] || 0) + 1;
                                }
                            });
                            const topPresent = Object.entries(presentByStudent)
                                .map(([studentId, count]) => {
                                    const student = students.find(s => s.id === studentId);
                                    return { student, count };
                                })
                                .filter(item => item.student)
                                .sort((a, b) => b.count - a.count)
                                .slice(0, 5);
                            
                            let presentHtml = '';
                            if (topPresent.length === 0) {
                                presentHtml = '<tr><td colspan="5" class="text-center">ไม่มีข้อมูล</td></tr>';
                            } else {
                                topPresent.forEach((item, index) => {
                                    presentHtml += `
                                        <tr>
                                            <td>${index + 1}</td>
                                            <td>${item.student.name}</td>
                                            <td>${item.student.class}</td>
                                            <td>${item.student.classroom || '-'}</td>
                                            <td>${item.count}</td>
                                        </tr>
                                    `;
                                });
                            }
                            document.getElementById('top-present').innerHTML = presentHtml;

                            const absentByStudent = {};
                            validAttendance.forEach(a => {
                                if (a.status === 'absent') {
                                    absentByStudent[a.studentId] = (absentByStudent[a.studentId] || 0) + 1;
                                }
                            });
                            const topAbsent = Object.entries(absentByStudent)
                                .map(([studentId, count]) => {
                                    const student = students.find(s => s.id === studentId);
                                    return { student, count };
                                })
                                .filter(item => item.student)
                                .sort((a, b) => b.count - a.count)
                                .slice(0, 5);
                            
                            let absentHtml = '';
                            if (topAbsent.length === 0) {
                                absentHtml = '<tr><td colspan="5" class="text-center">ไม่มีข้อมูล</td></tr>';
                            } else {
                                topAbsent.forEach((item, index) => {
                                    absentHtml += `
                                        <tr>
                                            <td>${index + 1}</td>
                                            <td>${item.student.name}</td>
                                            <td>${item.student.class}</td>
                                            <td>${item.student.classroom || '-'}</td>
                                            <td>${item.count}</td>
                                        </tr>
                                    `;
                                });
                            }
                            document.getElementById('top-absent').innerHTML = absentHtml;

                            hideLoading();
                            console.log('Dashboard loaded successfully');
                        })
                        .withFailureHandler(function(error) {
                            handleError(error);
                            console.error('Failed to load attendance data:', error);
                        })
                        .getData('attendance');
                })
                .withFailureHandler(function(error) {
                    handleError(error);
                    console.error('Failed to load student data:', error);
                })
                .getData('students');
        }

        // Subject Management
        document.getElementById('add-subject-btn').addEventListener('click', function() {
            document.getElementById('subject-modal-title').textContent = 'เพิ่มรายวิชา';
            document.getElementById('subject-id').value = '';
            document.getElementById('subject-code').value = '';
            document.getElementById('subject-name').value = '';
            openModal('subject-modal');
        });

        document.getElementById('subject-form').addEventListener('submit', function(e) {
            e.preventDefault();
            if (!$('#subject-form').valid()) {
                console.warn('Subject form validation failed');
                return;
            }

            const subjectId = document.getElementById('subject-id').value;
            const subjectCode = document.getElementById('subject-code').value;
            const subjectName = document.getElementById('subject-name').value;
            const data = { code: subjectCode, name: subjectName };

            showLoading();
            if (subjectId) {
                google.script.run
                    .withSuccessHandler(function() {
                        loadSubjects();
                        closeModal('subject-modal');
                        hideLoading();
                        Swal.fire({
                            icon: 'success',
                            title: 'สำเร็จ!',
                            text: 'อัปเดตรายวิชาเรียบร้อยแล้ว',
                            confirmButtonColor: '#FF69B4'
                        });
                        console.log('Subject updated:', subjectId);
                    })
                    .withFailureHandler(function(error) {
                        handleError(error);
                        console.error('Failed to update subject:', error);
                    })
                    .updateData('subjects', subjectId, data);
            } else {
                google.script.run
                    .withSuccessHandler(function() {
                        loadSubjects();
                        closeModal('subject-modal');
                        hideLoading();
                        Swal.fire({
                            icon: 'success',
                            title: 'สำเร็จ!',
                            text: 'เพิ่มรายวิชาเรียบร้อยแล้ว',
                            confirmButtonColor: '#FF69B4'
                        });
                        console.log('Subject added:', subjectCode);
                    })
                    .withFailureHandler(function(error) {
                        handleError(error);
                        console.error('Failed to add subject:', error);
                    })
                    .addData('subjects', data);
            }
        });

        document.addEventListener('click', function(e) {
            if (e.target.classList.contains('edit-subject')) {
                const subjectId = e.target.dataset.id;
                console.log('Editing subject:', subjectId);
                showLoading();
                google.script.run
                    .withSuccessHandler(function(subjects) {
                        const subject = subjects.find(s => s.id === subjectId);
                        if (subject) {
                            document.getElementById('subject-modal-title').textContent = 'แก้ไขรายวิชา';
                            document.getElementById('subject-id').value = subject.id;
                            document.getElementById('subject-code').value = subject.code;
                            document.getElementById('subject-name').value = subject.name;
                            openModal('subject-modal');
                            hideLoading();
                            console.log('Subject loaded for edit:', subject);
                        } else {
                            hideLoading();
                            Swal.fire({
                                icon: 'error',
                                title: 'ไม่พบข้อมูล',
                                text: 'ไม่พบรายวิชาที่ต้องการแก้ไข',
                                confirmButtonColor: '#FF69B4'
                            });
                        }
                    })
                    .withFailureHandler(function(error) {
                        handleError(error);
                        console.error('Failed to load subject for edit:', error);
                    })
                    .getData('subjects');
            }
        });

        document.addEventListener('click', function(e) {
            if (e.target.classList.contains('delete-subject')) {
                const subjectId = e.target.dataset.id;
                console.log('Deleting subject:', subjectId);
                Swal.fire({
                    title: 'ยืนยันการลบ?',
                    text: "เมื่อคุณลบรายวิชา นักเรียนที่ผูกกับรายวิชานี้จะถูกอัปเดต และข้อมูลการเช็คชื่อในรายวิชานี้จะถูกลบด้วย คุณแน่ใจแล้วใช่ไหม?",
                    icon: 'warning',
                    showCancelButton: true,
                    confirmButtonColor: '#FF69B4',
                    cancelButtonColor: '#6c757d',
                    confirmButtonText: 'ใช่, ลบเลย!',
                    cancelButtonText: 'ยกเลิก'
                }).then((result) => {
                    if (result.isConfirmed) {
                        showLoading();
                        google.script.run
                            .withSuccessHandler(function() {
                                google.script.run
                                    .withSuccessHandler(function(attendance) {
                                        const recordsToDelete = attendance.filter(a => a.subjectId === subjectId);
                                        if (recordsToDelete.length === 0) {
                                            finishSubjectDeletion();
                                            return;
                                        }

                                        let count = 0;
                                        recordsToDelete.forEach(record => {
                                            google.script.run
                                                .withSuccessHandler(() => {
                                                    count++;
                                                    if (count === recordsToDelete.length) {
                                                        finishSubjectDeletion();
                                                    }
                                                })
                                                .withFailureHandler(error => {
                                                    handleError(error);
                                                    console.error('Failed to delete attendance record:', error);
                                                })
                                                .deleteData('attendance', record.id);
                                        });
                                    })
                                    .withFailureHandler(function(error) {
                                        handleError(error);
                                        console.error('Failed to load attendance after deleting subject:', error);
                                    })
                                    .getData('attendance');
                            })
                            .withFailureHandler(function(error) {
                                handleError(error);
                                console.error('Failed to delete subject:', error);
                            })
                            .deleteData('subjects', subjectId);
                    }
                });

                function finishSubjectDeletion() {
                    loadSubjects();
                    loadStudents();
                    hideLoading();
                    Swal.fire({
                        icon: 'success',
                        title: 'ลบแล้ว!',
                        text: 'รายวิชาถูกลบและข้อมูลการเช็คชื่อที่เกี่ยวข้องถูกลบเรียบร้อยแล้ว',
                        confirmButtonColor: '#FF69B4'
                    });
                    console.log('Subject and related attendance deleted:', subjectId);
                }
            }
        });

        function loadSubjects() {
            console.log('Loading subjects...');
            showLoading();
            google.script.run
                .withSuccessHandler(function(subjects) {
                    const tableBody = document.querySelector('#subjects-table tbody');
                    let html = '';
                    if (!subjects || subjects.length === 0) {
                        html = '<tr><td colspan="3" class="text-center">ไม่มีข้อมูล</td></tr>';
                    } else {
                        subjects.forEach(subject => {
                            html += `
                                <tr>
                                    <td>${subject.code}</td>
                                    <td>${subject.name}</td>
                                    <td>
                                        <button class="edit-subject bg-yellow-500 hover:bg-yellow-600 text-white px-2 py-1 rounded mr-2" data-id="${subject.id}">แก้ไข</button>
                                        <button class="delete-subject bg-red-500 hover:bg-red-600 text-white px-2 py-1 rounded" data-id="${subject.id}">ลบ</button>
                                    </td>
                                </tr>
                            `;
                        });
                    }
                    tableBody.innerHTML = html;

                    const studentSubjectSelect = document.getElementById('student-subject');
                    const attendanceSubjectSelect = document.getElementById('attendance-subject');

                    let optionsHtmlStudent = '';
                    subjects.forEach(subject => {
                        optionsHtmlStudent += `<option value="${subject.id}">${subject.name}</option>`;
                    });
                    studentSubjectSelect.innerHTML = optionsHtmlStudent;

                    let optionsHtmlAttendance = '<option value="all" selected>ทุกรายวิชา</option>';
                    subjects.forEach(subject => {
                        optionsHtmlAttendance += `<option value="${subject.id}">${subject.name}</option>`;
                    });
                    attendanceSubjectSelect.innerHTML = optionsHtmlAttendance;

                    $('#student-subject').select2('destroy').select2({
                        placeholder: 'กรุณาเลือกรายวิชา',
                        allowClear: true,
                        width: '100%'
                    });

                    hideLoading();
                    console.log('Subjects loaded:', subjects.length);
                })
                .withFailureHandler(function(error) {
                    handleError(error);
                    console.error('Failed to load subjects:', error);
                })
                .getData('subjects');
        }

        // Function to populate classroom dropdowns
        function populateClassroomDropdowns(students) {
            console.log('Populating classroom dropdowns...');
            const attendanceClassroomSelect = document.getElementById('attendance-classroom');
            const reportClassroomSelect = document.getElementById('report-classroom');
            const classrooms = [...new Set(students.map(s => s.classroom).filter(c => c))].sort();
            let optionsHtml = '<option value="">ทุกห้องเรียน</option>';
            classrooms.forEach(classroom => {
                optionsHtml += `<option value="${classroom}">${classroom}</option>`;
            });
            attendanceClassroomSelect.innerHTML = optionsHtml;
            reportClassroomSelect.innerHTML = optionsHtml;
            console.log('Classrooms populated:', classrooms);
        }

        // Student Management
        document.getElementById('add-student-btn').addEventListener('click', function() {
            console.log('Opening add student modal');
            document.getElementById('student-modal-title').textContent = 'เพิ่มนักเรียน';
            document.getElementById('student-id').value = '';
            document.getElementById('student-code').value = '';
            document.getElementById('student-name').value = '';
            document.getElementById('student-class').value = '';
            document.getElementById('student-classroom').value = '';
            $('#student-subject').val(null).trigger('change');
            $('#student-subject').data('edit-mode', false);
            $('#student-subject').data('original-subjects', []);
            openModal('student-modal');
        });

        document.getElementById('student-form').addEventListener('submit', function(e) {
            e.preventDefault();
            console.log('Submitting student form');
            if (!$('#student-form').valid()) {
                console.warn('Student form validation failed');
                return;
            }

            const studentId = document.getElementById('student-id').value;
            const studentCode = document.getElementById('student-code').value;
            const studentName = document.getElementById('student-name').value;
            const studentClass = document.getElementById('student-class').value;
            const studentClassroom = document.getElementById('student-classroom').value;
            const studentSubjects = Array.from(document.getElementById('student-subject').selectedOptions)
                .map(option => option.value)
                .join(',');

            const data = {
                code: studentCode,
                name: studentName,
                class: studentClass,
                classroom: studentClassroom,
                subjectId: studentSubjects
            };

            showLoading();
            if (studentId) {
                google.script.run
                    .withSuccessHandler(function() {
                        loadStudents();
                        closeModal('student-modal');
                        hideLoading();
                        Swal.fire({
                            icon: 'success',
                            title: 'สำเร็จ!',
                            text: 'อัปเดตข้อมูลนักเรียนเรียบร้อยแล้ว',
                            confirmButtonColor: '#FF69B4'
                        });
                        console.log('Student updated:', studentId);
                    })
                    .withFailureHandler(function(error) {
                        handleError(error);
                        console.error('Failed to update student:', error);
                    })
                    .updateData('students', studentId, data);
            } else {
                google.script.run
                    .withSuccessHandler(function() {
                        loadStudents();
                        closeModal('student-modal');
                        hideLoading();
                        Swal.fire({
                            icon: 'success',
                            title: 'สำเร็จ!',
                            text: 'เพิ่มนักเรียนเรียบร้อยแล้ว',
                            confirmButtonColor: '#FF69B4'
                        });
                        console.log('Student added:', studentCode);
                    })
                    .withFailureHandler(function(error) {
                        handleError(error);
                        console.error('Failed to add student:', error);
                    })
                    .addData('students', data);
            }
        });

        document.addEventListener('click', function(e) {
            if (e.target.classList.contains('edit-student')) {
                const studentId = e.target.dataset.id;
                console.log('Editing student:', studentId);
                showLoading();
                google.script.run
                    .withSuccessHandler(function(students) {
                        const student = students.find(s => s.id === studentId);
                        if (student) {
                            document.getElementById('student-modal-title').textContent = 'แก้ไขข้อมูลนักเรียน';
                            document.getElementById('student-id').value = student.id;
                            document.getElementById('student-code').value = student.code;
                            document.getElementById('student-name').value = student.name;
                            document.getElementById('student-class').value = student.class;
                            document.getElementById('student-classroom').value = student.classroom || '';
                            const subjectIds = student.subjectId ? student.subjectId.split(',') : [];
                            $('#student-subject').val(subjectIds).trigger('change');

                            $('#student-subject').data('original-subjects', subjectIds);
                            $('#student-subject').data('edit-mode', true);

                            openModal('student-modal');
                            hideLoading();
                            console.log('Student loaded for edit:', student);
                        } else {
                            hideLoading();
                            Swal.fire({
                                icon: 'error',
                                title: 'ไม่พบข้อมูล',
                                text: 'ไม่พบนักเรียนที่ต้องการแก้ไข',
                                confirmButtonColor: '#FF69B4'
                            });
                        }
                    })
                    .withFailureHandler(function(error) {
                        handleError(error);
                        console.error('Failed to load student for edit:', error);
                    })
                    .getData('students');
            }
        });

        document.addEventListener('click', function(e) {
            if (e.target.classList.contains('delete-student')) {
                const studentId = e.target.dataset.id;
                console.log('Deleting student:', studentId);
                Swal.fire({
                    title: 'ยืนยันการลบ?',
                    text: "เมื่อคุณลบนักเรียนคนนี้ รายการเช็คเวลาเรียนของนักเรียนคนนี้จะถูกลบออกไปด้วย คุณแน่ใจแล้วใช่ไหม?",
                    icon: 'warning',
                    showCancelButton: true,
                    confirmButtonColor: '#FF69B4',
                    cancelButtonColor: '#6c757d',
                    confirmButtonText: 'ใช่, ลบเลย!',
                    cancelButtonText: 'ยกเลิก'
                }).then((result) => {
                    if (result.isConfirmed) {
                        showLoading();
                        google.script.run
                            .withSuccessHandler(function() {
                                loadStudents();
                                hideLoading();
                                Swal.fire({
                                    icon: 'success',
                                    title: 'ลบแล้ว!',
                                    text: 'ข้อมูลนักเรียนถูกลบเรียบร้อยแล้ว',
                                    confirmButtonColor: '#FF69B4'
                                });
                                console.log('Student deleted:', studentId);
                            })
                            .withFailureHandler(function(error) {
                                handleError(error);
                                console.error('Failed to delete student:', error);
                            })
                            .deleteData('students', studentId);
                    }
                });
            }
        });

        function loadStudents() {
            console.log('Loading students...');
            showLoading();
            google.script.run
                .withSuccessHandler(function(students) {
                    google.script.run
                        .withSuccessHandler(function(subjects) {
                            const tableBody = document.querySelector('#students-table tbody');
                            let html = '';
                            if (!Array.isArray(students) || students.length === 0) {
                                html = '<tr><td colspan="6" class="text-center">ไม่มีข้อมูลนักเรียน กรุณาเพิ่มข้อมูลนักเรียนก่อน</td></tr>';
                            } else {
                                students.forEach(student => {
                                    const subjectIds = student.subjectId ? student.subjectId.split(',') : [];
                                    const subjectNames = subjectIds
                                        .map(id => {
                                            const subject = subjects.find(s => s.id === id);
                                            return subject ? subject.name : '-';
                                        })
                                        .join(', ') || '-';
                                    html += `
                                        <tr>
                                            <td>${student.code}</td>
                                            <td>${student.name}</td>
                                            <td>${student.class}</td>
                                            <td>${student.classroom || '-'}</td>
                                            <td>${subjectNames}</td>
                                            <td>
                                                <button class="edit-student bg-yellow-500 hover:bg-yellow-600 text-white px-2 py-1 rounded mr-2" data-id="${student.id}">แก้ไข</button>
                                                <button class="delete-student bg-red-500 hover:bg-red-600 text-white px-2 py-1 rounded" data-id="${student.id}">ลบ</button>
                                            </td>
                                        </tr>
                                    `;
                                });
                            }
                            tableBody.innerHTML = html;

                            const reportStudentSelect = document.getElementById('report-student');
                            let optionsHtml = '<option value="">ทุกคน</option>';
                            if (!Array.isArray(students) || students.length === 0) {
                                optionsHtml = '<option value="">ไม่มีนักเรียนในระบบ</option>';
                            } else {
                                students.forEach(student => {
                                    if (student.id && student.name) {
                                        optionsHtml += `<option value="${student.id}">${student.name} (${student.class}/${student.classroom || '-'})</option>`;
                                    }
                                });
                            }
                            reportStudentSelect.innerHTML = optionsHtml;

                            populateClassroomDropdowns(students);

                            if (students.length === 0) {
                                Swal.fire({
                                    icon: 'warning',
                                    title: 'ไม่พบข้อมูลนักเรียน',
                                    text: 'กรุณาเพิ่มนักเรียนในแท็บ "จัดการนักเรียน" ก่อนสร้างรายงาน',
                                    confirmButtonColor: '#FF69B4'
                                });
                            }

                            hideLoading();
                            console.log('Students loaded:', students.length);
                        })
                        .withFailureHandler(function(error) {
                            handleError(error);
                            console.error('Failed to load subjects for students:', error);
                        })
                        .getData('subjects');
                })
                .withFailureHandler(function(error) {
                    handleError(error);
                    console.error('Failed to load students:', error);
                    const reportStudentSelect = document.getElementById('report-student');
                    reportStudentSelect.innerHTML = '<option value="">ไม่มีนักเรียนในระบบ</option>';
                    populateClassroomDropdowns([]);
                })
                .getData('students');
        }

        function loadAttendanceTable() {
            console.log('Loading attendance table...');
            const date = normalizeDate(document.getElementById('attendance-date').value);
            const subjectId = document.getElementById('attendance-subject').value;
            const classLevel = document.getElementById('attendance-class').value;
            const classroom = document.getElementById('attendance-classroom').value;
            if (!date) {
                Swal.fire({
                    icon: 'error',
                    title: 'ข้อมูลไม่ครบถ้วน',
                    text: 'กรุณาเลือกวันที่',
                    confirmButtonColor: '#FF69B4'
                });
                return;
            }
            showLoading();
            google.script.run
                .withSuccessHandler(function(students) {
                    google.script.run
                        .withSuccessHandler(function(subjects) {
                            google.script.run
                                .withSuccessHandler(function(attendance) {
                                    currentAttendance = Array.isArray(attendance) ? attendance.map(record => ({
                                        ...record,
                                        date: normalizeDate(record.date)
                                    })) : [];

                                    let filteredStudents = students;
                                    if (classLevel !== 'all') {
                                        filteredStudents = filteredStudents.filter(s => s.class === classLevel);
                                    }
                                    if (classroom) {
                                        filteredStudents = filteredStudents.filter(s => s.classroom === classroom);
                                    }

                                    let rows = [];

                                    if (subjectId !== 'all') {
                                        filteredStudents = filteredStudents.filter(s => {
                                            const subjectIds = s.subjectId ? s.subjectId.split(',') : [];
                                            return subjectIds.includes(subjectId);
                                        });
                                        rows = filteredStudents.map(student => {
                                            return {
                                                student,
                                                subjectId: subjectId
                                            };
                                        });
                                    } else {
                                        filteredStudents.forEach(student => {
                                            const subjectIds = student.subjectId ? student.subjectId.split(',') : [];
                                            subjectIds.forEach(subId => {
                                                rows.push({
                                                    student,
                                                    subjectId: subId
                                                });
                                            });
                                        });
                                    }

                                    if (rows.length === 0) {
                                        hideLoading();
                                        Swal.fire({
                                            icon: 'info',
                                            title: 'ไม่พบข้อมูล',
                                            text: 'ไม่พบนักเรียนในระดับชั้น, ห้องเรียน, และรายวิชาที่เลือก',
                                            confirmButtonColor: '#FF69B4'
                                        });
                                        document.getElementById('attendance-list').classList.add('hidden');
                                        return;
                                    }

                                    const tableBody = document.querySelector('#attendance-table tbody');
                                    let html = '';
                                    rows.forEach(({ student, subjectId }) => {
                                        const subject = subjects.find(s => s.id === subjectId);
                                        const existingAttendance = currentAttendance.find(a =>
                                            a.date === date &&
                                            a.studentId.toString() === student.id.toString() &&
                                            a.subjectId.toString() === subjectId.toString()
                                        );
                                        const status = existingAttendance ? existingAttendance.status : 'none';
                                        const statusText = { 'none': 'ยังไม่เช็ค', 'present': 'มา', 'absent': 'ขาด', 'leave': 'ลา', 'late': 'สาย' };
                                        const statusClass = { 'none': 'attendance-none', 'present': 'attendance-present', 'absent': 'attendance-absent', 'leave': 'attendance-leave', 'late': 'attendance-late' };
                                        html += `
                                            <tr data-student-id="${student.id}" data-subject-id="${subjectId}" data-date="${date}" data-student-name="${student.name}" data-class="${student.class}" data-classroom="${student.classroom || ''}">
                                                <td>${student.code || '-'}</td>
                                                <td>${student.name}</td>
                                                <td>${student.class}</td>
                                                <td>${student.classroom || '-'}</td>
                                                <td>${subject ? subject.name : '-'}</td>
                                                <td><span class="${statusClass[status]} px-2 py-1 rounded" data-status="${status}">${statusText[status]}</span></td>
                                                <td>
                                                    <button class="mark-present bg-green-500 hover:bg-green-600 text-white px-2 py-1 rounded mr-1" data-id="${student.id}">มา</button>
                                                    <button class="mark-absent bg-red-500 hover:bg-red-600 text-white px-2 py-1 rounded mr-1" data-id="${student.id}">ขาด</button>
                                                    <button class="mark-leave bg-yellow-500 hover:bg-yellow-600 text-white px-2 py-1 rounded mr-1" data-id="${student.id}">ลา</button>
                                                    <button class="mark-late bg-pink-500 hover:bg-pink-600 text-white px-2 py-1 rounded" data-id="${student.id}">สาย</button>
                                                </td>
                                            </tr>
                                        `;
                                    });
                                    tableBody.innerHTML = html;
                                    document.getElementById('attendance-list').classList.remove('hidden');
                                    hideLoading();
                                    console.log('Attendance table loaded:', rows.length);
                                })
                                .withFailureHandler(function(error) {
                                    handleError(error);
                                    console.error('Failed to load attendance data:', error);
                                })
                                .getData('attendance');
                        })
                        .withFailureHandler(function(error) {
                            handleError(error);
                            console.error('Failed to load subjects for attendance:', error);
                        })
                        .getData('subjects');
                })
                .withFailureHandler(function(error) {
                    handleError(error);
                    console.error('Failed to load students for attendance:', error);
                })
                .getData('students');
        }

        document.getElementById('start-attendance').addEventListener('click', function() {
            console.log('Starting attendance...');
            loadAttendanceTable();
        });

        document.addEventListener('click', function(e) {
            if (e.target.classList.contains('mark-present') || e.target.classList.contains('mark-absent') || e.target.classList.contains('mark-leave') || e.target.classList.contains('mark-late')) {
                const studentId = e.target.dataset.id;
                const row = e.target.closest('tr');
                const statusCell = row.querySelector('td:nth-child(6) span');
                let status;
                if (e.target.classList.contains('mark-present')) status = 'present';
                else if (e.target.classList.contains('mark-absent')) status = 'absent';
                else if (e.target.classList.contains('mark-leave')) status = 'leave';
                else if (e.target.classList.contains('mark-late')) status = 'late';
                const statusText = { 'present': 'มา', 'absent': 'ขาด', 'leave': 'ลา', 'late': 'สาย' };
                const statusClass = { 'present': 'attendance-present', 'absent': 'attendance-absent', 'leave': 'attendance-leave', 'late': 'attendance-late' };
                statusCell.textContent = statusText[status];
                statusCell.className = `${statusClass[status]} px-2 py-1 rounded`;
                statusCell.dataset.status = status;
                console.log(`Marked attendance for student ${studentId}: ${status}`);
            }
        });

        document.getElementById('mark-all-present').addEventListener('click', function() {
            console.log('Marking all present...');
            const rows = document.querySelectorAll('#attendance-table tbody tr');
            rows.forEach(row => {
                const statusCell = row.querySelector('td:nth-child(6) span');
                statusCell.textContent = 'มา';
                statusCell.className = 'attendance-present px-2 py-1 rounded';
                statusCell.dataset.status = 'present';
            });
        });

        document.getElementById('reset-attendance').addEventListener('click', function() {
            console.log('Resetting attendance...');
            const rows = document.querySelectorAll('#attendance-table tbody tr');
            rows.forEach(row => {
                const statusCell = row.querySelector('td:nth-child(6) span');
                statusCell.textContent = 'ยังไม่เช็ค';
                statusCell.className = 'attendance-none px-2 py-1 rounded';
                statusCell.dataset.status = 'none';
            });
        });

        document.getElementById('save-attendance').addEventListener('click', function() {
            console.log('Saving attendance (batch)...');
            const rows = document.querySelectorAll('#attendance-table tbody tr');
            const saveRecords = [];

            rows.forEach(row => {
                const studentId = row.dataset.studentId;
                const subjectId = row.dataset.subjectId;
                const date = row.dataset.date;
                const studentName = row.dataset.studentName;
                const classLevel = row.dataset.class;
                const classroom = row.dataset.classroom;
                const status = row.querySelector('td:nth-child(6) span').dataset.status;

                if (status && status !== 'none') {
                    saveRecords.push({
                        studentId,
                        subjectId,
                        date,
                        status,
                        studentName,
                        class: classLevel,
                        classroom
                    });
                }
            });

            if (saveRecords.length === 0) {
                Swal.fire({
                    icon: 'warning',
                    title: 'ไม่มีข้อมูล',
                    text: 'กรุณาเลือกสถานะสำหรับนักเรียนอย่างน้อยหนึ่งคน',
                    confirmButtonColor: '#FF69B4'
                });
                return;
            }

            showLoading();
            google.script.run
                .withSuccessHandler(() => {
                    hideLoading();
                    document.getElementById('attendance-list').classList.add('hidden');
                    currentAttendance = [];
                    Swal.fire({
                        icon: 'success',
                        title: 'สำเร็จ!',
                        text: 'บันทึกเวลาเรียนเรียบร้อยแล้ว',
                        confirmButtonColor: '#FF69B4'
                    });
                    loadDashboard();
                    console.log('Batch attendance saved');
                })
                .withFailureHandler(error => {
                    handleError(error);
                    console.error('Batch save failed:', error);
                })
                .saveAllAttendance(saveRecords);
        });

        function downloadCSV(csv, filename) {
            const BOM = '\uFEFF';
            const csvFile = new Blob([BOM + csv], { type: 'text/csv;charset=utf-8;' });
            const link = document.createElement('a');
            link.href = URL.createObjectURL(csvFile);
            link.download = filename;
            document.body.appendChild(link);
            link.click();
            document.body.removeChild(link);
        }

        function exportTableToCSV(filename) {
            console.log('Exporting table to CSV...');
            const rows = document.querySelectorAll("#report-table tr");
            let csv = [];
            rows.forEach(row => {
                const cols = row.querySelectorAll("th, td");
                const rowData = [];
                cols.forEach(col => {
                    let text = col.innerText.replace(/"/g, '""');
                    if (text.includes(",") || text.includes('"')) {
                        text = `"${text}"`;
                    }
                    rowData.push(text);
                });
                csv.push(rowData.join(","));
            });
            downloadCSV(csv.join("\n"), filename);
            console.log('CSV exported:', filename);
        }

        document.getElementById("export-csv").addEventListener("click", function() {
            exportTableToCSV("รายงานเช็คชื่อ.csv");
        });

        document.getElementById("export-xlsx").addEventListener("click", function() {
            console.log('Exporting table to XLSX...');
            const table = document.getElementById("report-table");
            const wb = XLSX.utils.table_to_book(table, { sheet: "รายงานเช็คชื่อ" });
            XLSX.writeFile(wb, "รายงานเช็คชื่อ.xlsx");
            console.log('XLSX exported');
        });

        // Report Generation
        document.getElementById('generate-report').addEventListener('click', function() {
            console.log('Generating report...');
            const startDate = normalizeDate(document.getElementById('report-start-date').value);
            const endDate = normalizeDate(document.getElementById('report-end-date').value);
            const classLevel = document.getElementById('report-class').value;
            const classroom = document.getElementById('report-classroom').value;
            const studentId = document.getElementById('report-student').value;

            if (!startDate || !endDate) {
                Swal.fire({
                    icon: 'error',
                    title: 'ข้อมูลไม่ครบถ้วน',
                    text: 'กรุณาเลือกวันที่เริ่มต้นและวันที่สิ้นสุด',
                    confirmButtonColor: '#FF69B4'
                });
                return;
            }

            const start = new Date(startDate);
            const end = new Date(endDate);
            if (start > end) {
                Swal.fire({
                    icon: 'error',
                    title: 'วันที่ไม่ถูกต้อง',
                    text: 'วันที่เริ่มต้นต้องไม่หลังวันที่สิ้นสุด',
                    confirmButtonColor: '#FF69B4'
                });
                return;
            }

            showLoading();
            google.script.run
                .withSuccessHandler(function(attendance) {
                    google.script.run
                        .withSuccessHandler(function(subjects) {
                            google.script.run
                                .withSuccessHandler(function(students) {
                                    if (!Array.isArray(attendance) || attendance.length === 0) {
                                        hideLoading();
                                        Swal.fire({
                                            icon: 'warning',
                                            title: 'ไม่พบข้อมูลการเช็คชื่อ',
                                            text: 'กรุณาเพิ่มข้อมูลการเช็คชื่อในแท็บ "จัดการเช็คเวลาเรียน"',
                                            confirmButtonColor: '#FF69B4'
                                        });
                                        document.getElementById('report-results').classList.add('hidden');
                                        return;
                                    }

                                    let filteredAttendance = attendance.map(record => ({
                                        ...record,
                                        date: normalizeDate(record.date)
                                    })).filter(record => {
                                        const recordDate = new Date(record.date);
                                        return recordDate >= start && recordDate <= end;
                                    });

                                    if (classLevel) {
                                        filteredAttendance = filteredAttendance.filter(a => a.class === classLevel);
                                    }
                                    if (classroom) {
                                        filteredAttendance = filteredAttendance.filter(a => a.classroom === classroom);
                                    }
                                    if (studentId) {
                                        filteredAttendance = filteredAttendance.filter(a => a.studentId.toString() === studentId.toString());
                                    }

                                    const tableBody = document.querySelector('#report-table tbody');
                                    let html = '';
                                    if (filteredAttendance.length === 0) {
                                        html = '<tr><td colspan="7" class="text-center">ไม่มีข้อมูล</td></tr>';
                                    } else {
                                        filteredAttendance.forEach(record => {
                                            const subject = subjects.find(s => s.id.toString() === record.subjectId.toString());
                                            const statusText = {
                                                'present': 'มา',
                                                'absent': 'ขาด',
                                                'leave': 'ลา',
                                                'late': 'สาย'
                                            }[record.status] || '-';
                                            const statusClass = {
                                                'present': 'attendance-present',
                                                'absent': 'attendance-absent',
                                                'leave': 'attendance-leave',
                                                'late': 'attendance-late'
                                            }[record.status] || 'attendance-none';
                                            html += `
                                                <tr>
                                                    <td>${record.date}</td>
                                                    <td>${students.find(s => s.id.toString() === record.studentId.toString())?.code || '-'}</td>
                                                    <td>${record.studentName}</td>
                                                    <td>${record.class}</td>
                                                    <td>${record.classroom || '-'}</td>
                                                    <td>${subject ? subject.name : '-'}</td>
                                                    <td><span class="${statusClass} px-2 py-1 rounded">${statusText}</span></td>
                                                </tr>
                                            `;
                                        });
                                    }
                                    tableBody.innerHTML = html;

                                    const totalStudents = new Set(filteredAttendance.map(a => a.studentId)).size;
                                    const presentCount = filteredAttendance.filter(a => a.status === 'present').length;
                                    const absentCount = filteredAttendance.filter(a => a.status === 'absent').length;
                                    const leaveCount = filteredAttendance.filter(a => a.status === 'leave').length;
                                    const lateCount = filteredAttendance.filter(a => a.status === 'late').length;

                                    document.getElementById('report-total-students').textContent = totalStudents;
                                    document.getElementById('report-present-students').textContent = presentCount;
                                    document.getElementById('report-absent-students').textContent = absentCount;
                                    document.getElementById('report-leave-students').textContent = leaveCount;
                                    document.getElementById('report-late-students').textContent = lateCount;

                                    document.getElementById('report-results').classList.remove('hidden');
                                    hideLoading();
                                    console.log('Report generated:', filteredAttendance.length, 'records');
                                })
                                .withFailureHandler(function(error) {
                                    handleError(error);
                                    console.error('Failed to load students for report:', error);
                                })
                                .getData('students');
                        })
                        .withFailureHandler(function(error) {
                            handleError(error);
                            console.error('Failed to load subjects for report:', error);
                        })
                        .getData('subjects');
                })
                .withFailureHandler(function(error) {
                    handleError(error);
                    console.error('Failed to load attendance for report:', error);
                })
                .getData('attendance');
        });

        // Download CSV Template Functions
        function downloadCSVTemplate(sheetName, headers, filename) {
            const csv = headers.join(',') + '\n';
            const BOM = '\uFEFF';
            const csvFile = new Blob([BOM + csv], { type: 'text/csv;charset=utf-8;' });
            const link = document.createElement('a');
            link.href = URL.createObjectURL(csvFile);
            link.download = filename;
            document.body.appendChild(link);
            link.click();
            document.body.removeChild(link);
            console.log(`Downloaded ${filename}`);
        }

        document.getElementById('download-subjects-template').addEventListener('click', function() {
            const headers = ['code', 'name'];
            downloadCSVTemplate('subjects', headers, 'template_subjects.csv');
        });

        document.getElementById('download-students-template').addEventListener('click', function() {
            const headers = ['code', 'name', 'class', 'classroom', 'subjectId'];
            downloadCSVTemplate('students', headers, 'template_students.csv');
        });

        // Import Subjects
        document.getElementById('import-subjects-btn').addEventListener('click', function() {
            document.getElementById('subjects-csv-file').value = '';
            openModal('import-subjects-modal');
        });

        document.getElementById('import-subjects-form').addEventListener('submit', function(e) {
            e.preventDefault();
            const fileInput = document.getElementById('subjects-csv-file');
            if (!fileInput.files.length) {
                Swal.fire({
                    icon: 'error',
                    title: 'ไม่พบไฟล์',
                    text: 'กรุณาเลือกไฟล์ CSV',
                    confirmButtonColor: '#FF69B4'
                });
                return;
            }

            const file = fileInput.files[0];
            const reader = new FileReader();
            reader.onload = function(e) {
                const csvContent = e.target.result;
                showLoading();
                google.script.run
                    .withSuccessHandler(function(result) {
                        loadSubjects();
                        closeModal('import-subjects-modal');
                        hideLoading();
                        Swal.fire({
                            icon: 'success',
                            title: 'สำเร็จ!',
                            text: `นำเข้ารายวิชา ${result.count} รายการเรียบร้อยแล้ว`,
                            confirmButtonColor: '#FF69B4'
                        });
                        console.log('Imported subjects:', result);
                    })
                    .withFailureHandler(function(error) {
                        handleError(error);
                        console.error('Failed to import subjects:', error);
                    })
                    .importCSV('subjects', csvContent);
            };
            reader.readAsText(file, 'UTF-8');
        });

        // Import Students
        document.getElementById('import-students-btn').addEventListener('click', function() {
            document.getElementById('students-csv-file').value = '';
            openModal('import-students-modal');
        });

        document.getElementById('import-students-form').addEventListener('submit', function(e) {
            e.preventDefault();
            const fileInput = document.getElementById('students-csv-file');
            if (!fileInput.files.length) {
                Swal.fire({
                    icon: 'error',
                    title: 'ไม่พบไฟล์',
                    text: 'กรุณาเลือกไฟล์ CSV',
                    confirmButtonColor: '#FF69B4'
                });
                return;
            }

            const file = fileInput.files[0];
            const reader = new FileReader();
            reader.onload = function(e) {
                const csvContent = e.target.result;
                showLoading();
                google.script.run
                    .withSuccessHandler(function(result) {
                        loadStudents();
                        closeModal('import-students-modal');
                        hideLoading();
                        Swal.fire({
                            icon: 'success',
                            title: 'สำเร็จ!',
                            text: `นำเข้านักเรียน ${result.count} รายการเรียบร้อยแล้ว`,
                            confirmButtonColor: '#FF69B4'
                        });
                        console.log('Imported students:', result);
                    })
                    .withFailureHandler(function(error) {
                        handleError(error);
                        console.error('Failed to import students:', error);
                    })
                    .importCSV('students', csvContent);
            };
            reader.readAsText(file, 'UTF-8');
        });

        // Form Validations
        $(document).ready(function() {
            console.log('Initializing form validations...');

            // Subject Form Validation
            $('#subject-form').validate({
                rules: {
                    'subject-code': {
                        required: true,
                        minlength: 3,
                        maxlength: 10
                    },
                    'subject-name': {
                        required: true,
                        minlength: 3,
                        maxlength: 100
                    }
                },
                messages: {
                    'subject-code': {
                        required: 'กรุณากรอกรหัสวิชา',
                        minlength: 'รหัสวิชาต้องมีอย่างน้อย 3 ตัวอักษร',
                        maxlength: 'รหัสวิชาต้องไม่เกิน 10 ตัวอักษร'
                    },
                    'subject-name': {
                        required: 'กรุณากรอกชื่อรายวิชา',
                        minlength: 'ชื่อรายวิชาต้องมีอย่างน้อย 3 ตัวอักษร',
                        maxlength: 'ชื่อรายวิชาต้องไม่เกิน 100 ตัวอักษร'
                    }
                },
                errorElement: 'div',
                errorPlacement: function(error, element) {
                    error.addClass('error');
                    error.insertAfter(element);
                },
                highlight: function(element) {
                    $(element).addClass('border-red-500').removeClass('border-gray-300');
                },
                unhighlight: function(element) {
                    $(element).removeClass('border-red-500').addClass('border-gray-300');
                }
            });

            // Student Form Validation
            $('#student-form').validate({
                rules: {
                    'student-code': {
                        required: true,
                        minlength: 3,
                        maxlength: 20
                    },
                    'student-name': {
                        required: true,
                        minlength: 3,
                        maxlength: 100
                    },
                    'student-class': {
                        required: true
                    },
                    'student-classroom': {
                        required: true,
                        minlength: 1,
                        maxlength: 10
                    },
                    'student-subject': {
                        required: true
                    }
                },
                messages: {
                    'student-code': {
                        required: 'กรุณากรอกรหัสนักเรียน',
                        minlength: 'รหัสนักเรียนต้องมีอย่างน้อย 3 ตัวอักษร',
                        maxlength: 'รหัสนักเรียนต้องไม่เกิน 20 ตัวอักษร'
                    },
                    'student-name': {
                        required: 'กรุณากรอกชื่อ-นามสกุล',
                        minlength: 'ชื่อ-นามสกุลต้องมีอย่างน้อย 3 ตัวอักษร',
                        maxlength: 'ชื่อ-นามสกุลต้องไม่เกิน 100 ตัวอักษร'
                    },
                    'student-class': {
                        required: 'กรุณาเลือกระดับชั้น'
                    },
                    'student-classroom': {
                        required: 'กรุณากรอกห้องเรียน',
                        minlength: 'ห้องเรียนต้องมีอย่างน้อย 1 ตัวอักษร',
                        maxlength: 'ห้องเรียนต้องไม่เกิน 10 ตัวอักษร'
                    },
                    'student-subject': {
                        required: 'กรุณาเลือกรายวิชาอย่างน้อยหนึ่งรายวิชา'
                    }
                },
                errorElement: 'div',
                errorPlacement: function(error, element) {
                    error.addClass('error');
                    if (element.attr('name') === 'student-subject') {
                        error.insertAfter(element.next('.select2-container'));
                    } else {
                        error.insertAfter(element);
                    }
                },
                highlight: function(element) {
                    $(element).addClass('border-red-500').removeClass('border-gray-300');
                },
                unhighlight: function(element) {
                    $(element).removeClass('border-red-500').addClass('border-gray-300');
                }
            });

            // Initialize Select2 for student-subject with deselection prevention
            $('#student-subject').select2({
                placeholder: 'กรุณาเลือกรายวิชา',
                allowClear: true,
                width: '100%'
            }).on('select2:unselecting', function(e) {
                if ($(this).data('edit-mode')) {
                    const originalSubjects = $(this).data('original-subjects') || [];
                    const selectedValue = e.params.args.data.id;
                    if (originalSubjects.includes(selectedValue)) {
                        Swal.fire({
                            icon: 'warning',
                            title: 'ไม่สามารถลบรายวิชาได้',
                            text: 'กรุณาลบรายวิชาในแท็บ "จัดการรายวิชา"',
                            confirmButtonColor: '#FF69B4'
                        });
                        e.preventDefault();
                    }
                }
            }).on('change', function() {
                $(this).valid();
            });

            // Set default date for attendance
            document.getElementById('attendance-date').value = normalizeDate(new Date());
            document.getElementById('report-end-date').value = normalizeDate(new Date());
            const oneWeekAgo = new Date();
            oneWeekAgo.setDate(oneWeekAgo.getDate() - 7);
            document.getElementById('report-start-date').value = normalizeDate(oneWeekAgo);

            // Initial loads
            loadDashboard();
            loadSubjects();
            loadStudents();

            initializeTabs();
            console.log('Application initialized');
        });
    </script>
</body>
</html>
