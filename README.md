<!DOCTYPE html>
<html lang="th">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>ระบบเช็คชื่อด้วยคิวอาร์โค้ด</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <!-- Font -->
    <link href="https://fonts.googleapis.com/css2?family=Prompt:wght@300;400;500;600;700&display=swap" rel="stylesheet">
    <!-- FontAwesome Icons -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <!-- QRCode Generator Library -->
    <script src="https://cdn.jsdelivr.net/npm/qrcode-generator@1.4.4/qrcode.min.js"></script>
    <style>
        body {
            font-family: 'Prompt', sans-serif;
        }
    </style>
</head>
<body class="bg-slate-50 min-h-screen text-slate-800 flex flex-col justify-between">

    <header class="bg-white shadow-sm sticky top-0 z-50">
        <div class="max-w-6xl mx-auto px-4 py-4 flex flex-col sm:flex-row justify-between items-center gap-4">
            <div class="flex items-center space-x-3">
                <div class="bg-blue-600 text-white p-2.5 rounded-xl shadow-md">
                    <i class="fa-solid fa-qrcode text-xl"></i>
                </div>
                <div>
                    <h1 class="text-xl font-bold text-slate-900">ระบบเช็คชื่อด้วยคิวอาร์โค้ด</h1>
                    <p class="text-xs text-slate-500">ตามแผนผังการทำงาน (เลือกห้องก่อนเปิดระบบ & QR ใช้งานได้จริง)</p>
                </div>
            </div>
            <!-- Role Switcher -->
            <div class="flex bg-slate-100 p-1 rounded-xl shadow-inner">
                <button onclick="switchRole('teacher')" id="btn-teacher" class="px-5 py-2 rounded-lg font-medium text-sm transition-all bg-white text-blue-600 shadow-sm">
                    <i class="fa-solid fa-chalkboard-user mr-2"></i>คอมของครู
                </button>
                <button onclick="switchRole('student')" id="btn-student" class="px-5 py-2 rounded-lg font-medium text-sm transition-all text-slate-600 hover:text-slate-900">
                    <i class="fa-solid fa-mobile-screen-button mr-2"></i>โทรศัพท์นักเรียน
                </button>
            </div>
        </div>
    </header>

    <main class="max-w-6xl mx-auto px-4 py-8 flex-grow w-full">

        <!-- ==================== TEACHER VIEW ==================== -->
        <section id="view-teacher" class="space-y-6">
            <div class="bg-gradient-to-r from-blue-600 to-indigo-700 rounded-2xl p-6 text-white shadow-xl flex flex-col md:flex-row justify-between items-center gap-6">
                <div>
                    <span class="bg-blue-500/30 text-blue-100 text-xs font-semibold px-3 py-1 rounded-full uppercase tracking-wider">Dashboard ครูผู้สอน</span>
                    <h2 class="text-2xl font-bold mt-2">ควบคุมระบบและเลือกห้องเรียน</h2>
                    <p class="text-blue-100 text-sm mt-1">เลือกห้องเรียนที่ต้องการเปิดระบบสแกนเช็คชื่อ เพื่อสร้าง QR Code ประจำห้อง</p>
                </div>
                <!-- Room Selection for System Activation -->
                <div class="bg-white/10 backdrop-blur-md p-4 rounded-xl border border-white/20 flex flex-col gap-3 min-w-[280px]">
                    <div>
                        <label class="block text-xs text-blue-200 mb-1 font-medium">เลือกห้องเรียน/วิชา ที่ต้องการเปิดระบบ:</label>
                        <select id="activeRoomSelect" onchange="onActiveRoomChange()" class="w-full bg-white text-slate-800 px-3 py-2 rounded-lg text-sm font-medium focus:outline-none focus:ring-2 focus:ring-blue-400">
                            <option value="">-- กรุณาเลือกห้องเรียน --</option>
                        </select>
                    </div>
                    <div class="flex items-center justify-between">
                        <span id="system-status-text" class="text-xs font-bold text-slate-200 flex items-center gap-1.5">
                            <span class="w-2.5 h-2.5 rounded-full bg-slate-400"></span> ปิดใช้งาน
                        </span>
                        <button onclick="toggleSystemStatus()" id="toggle-sys-btn" class="bg-emerald-500 hover:bg-emerald-600 text-white px-4 py-2 rounded-lg text-xs font-semibold shadow-md transition-all">
                            เปิดระบบเช็คชื่อ
                        </button>
                    </div>
                </div>
            </div>

            <!-- Teacher Grid Actions -->
            <div class="grid grid-cols-1 md:grid-cols-3 gap-6">
                
                <!-- 1. Add Class/Subject -->
                <div class="bg-white p-6 rounded-2xl shadow-sm border border-slate-100 flex flex-col justify-between">
                    <div>
                        <div class="w-10 h-10 rounded-xl bg-blue-50 text-blue-600 flex items-center justify-center font-bold mb-4">
                            <i class="fa-solid fa-book"></i>
                        </div>
                        <h3 class="font-bold text-lg text-slate-800">เพิ่มห้องเรียน / วิชาเรียน</h3>
                        <p class="text-xs text-slate-500 mt-1">กำหนดรายวิชาและห้องเรียนสำหรับเช็คชื่อ</p>
                        <form onsubmit="addClass(event)" class="mt-4 space-y-3">
                            <input type="text" id="classNameInput" placeholder="เช่น ม.3/1 วิชาคณิตศาสตร์" required class="w-full px-4 py-2.5 text-sm rounded-xl border border-slate-200 focus:outline-none focus:ring-2 focus:ring-blue-500">
                            <button type="submit" class="w-full bg-blue-600 hover:bg-blue-700 text-white font-medium py-2.5 rounded-xl text-sm transition-all shadow-sm">
                                เพิ่มห้องเรียน
                            </button>
                        </form>
                    </div>
                    <div class="mt-6 pt-4 border-t border-slate-100">
                        <p class="text-xs font-semibold text-slate-500 mb-2">ห้องเรียนทั้งหมด:</p>
                        <div id="class-list" class="flex flex-wrap gap-1.5 max-h-24 overflow-y-auto">
                            <span class="text-xs text-slate-400 italic">ยังไม่มีห้องเรียน</span>
                        </div>
                    </div>
                </div>

                <!-- 2. Add Student -->
                <div class="bg-white p-6 rounded-2xl shadow-sm border border-slate-100 flex flex-col justify-between">
                    <div>
                        <div class="w-10 h-10 rounded-xl bg-indigo-50 text-indigo-600 flex items-center justify-center font-bold mb-4">
                            <i class="fa-solid fa-user-plus"></i>
                        </div>
                        <h3 class="font-bold text-lg text-slate-800">เพิ่มนักเรียน</h3>
                        <p class="text-xs text-slate-500 mt-1">ระบุ ชื่อ, ชั้น, และเลขที่</p>
                        <form onsubmit="addStudent(event)" class="mt-4 space-y-3">
                            <select id="studentClassSelect" required class="w-full px-4 py-2 text-sm rounded-xl border border-slate-200 focus:outline-none focus:ring-2 focus:ring-indigo-500 bg-white">
                                <option value="">เลือกห้องเรียน</option>
                            </select>
                            <input type="text" id="studentNameInput" placeholder="ชื่อ - นามสกุล" required class="w-full px-4 py-2 text-sm rounded-xl border border-slate-200 focus:outline-none focus:ring-2 focus:ring-indigo-500">
                            <div class="grid grid-cols-2 gap-2">
                                <input type="text" id="studentLevelInput" placeholder="ชั้น (เช่น ม.3/1)" required class="px-4 py-2 text-sm rounded-xl border border-slate-200 focus:outline-none focus:ring-2 focus:ring-indigo-500">
                                <input type="number" id="studentNoInput" placeholder="เลขที่" required class="px-4 py-2 text-sm rounded-xl border border-slate-200 focus:outline-none focus:ring-2 focus:ring-indigo-500">
                            </div>
                            <button type="submit" class="w-full bg-indigo-600 hover:bg-indigo-700 text-white font-medium py-2 rounded-xl text-sm transition-all shadow-sm">
                                บันทึกนักเรียน
                            </button>
                        </form>
                    </div>
                </div>

                <!-- 3. Dynamic Working QR Code Display -->
                <div class="bg-white p-6 rounded-2xl shadow-sm border border-slate-100 flex flex-col items-center justify-center text-center">
                    <div class="w-10 h-10 rounded-xl bg-emerald-50 text-emerald-600 flex items-center justify-center font-bold mb-2">
                        <i class="fa-solid fa-qrcode"></i>
                    </div>
                    <h3 class="font-bold text-base text-slate-800">คิวอาร์โค้ดประจำห้อง</h3>
                    <p id="qr-hint" class="text-xs text-slate-400 mt-0.5">กรุณาเลือกห้องและเปิดระบบเพื่อสร้าง QR Code</p>
                    
                    <!-- QR Code Render Container -->
                    <div id="qrcode-container" class="my-3 p-3 bg-slate-50 rounded-2xl border border-slate-200 inline-block shadow-inner flex items-center justify-center min-h-[140px] min-w-[140px]">
                        <span class="text-xs text-slate-400">ยังไม่มี QR Code</span>
                    </div>
                    <span id="qr-room-badge" class="text-xs bg-slate-100 text-slate-500 font-medium px-3 py-1 rounded-full">-</span>
                </div>

            </div>

            <!-- Attendance Records Table -->
            <div class="bg-white p-6 rounded-2xl shadow-sm border border-slate-100">
                <div class="flex justify-between items-center mb-4">
                    <div>
                        <h3 class="font-bold text-lg text-slate-800">บันทึกสถิติการเข้าเรียนของนักเรียน</h3>
                        <p class="text-xs text-slate-500">รายงานผลการเช็คชื่อแบบเรียลไทม์ตามชั้นเรียน</p>
                    </div>
                    <button onclick="finishTeacherProcess()" class="bg-slate-800 hover:bg-slate-900 text-white text-xs font-semibold px-4 py-2 rounded-xl transition-all shadow-sm flex items-center gap-2">
                        <i class="fa-solid fa-check-circle"></i> เสร็จสิ้นกระบวนการครู
                    </button>
                </div>
                <div class="overflow-x-auto">
                    <table class="w-full text-left border-collapse">
                        <thead>
                            <tr class="bg-slate-50 text-slate-500 text-xs uppercase tracking-wider border-b border-slate-200">
                                <th class="py-3 px-4 rounded-l-xl">เลขที่</th>
                                <th class="py-3 px-4">ชื่อ - นามสกุล</th>
                                <th class="py-3 px-4">ชั้น/วิชา</th>
                                <th class="py-3 px-4">เวลาที่เช็คชื่อ</th>
                                <th class="py-3 px-4 rounded-r-xl">สถานะ</th>
                            </tr>
                        </thead>
                        <tbody id="attendance-table-body" class="text-sm divide-y divide-slate-100">
                            <tr>
                                <td colspan="5" class="py-6 text-center text-slate-400 italic">ยังไม่มีข้อมูลการเช็คชื่อในระบบ</td>
                            </tr>
                        </tbody>
                    </table>
                </div>
            </div>
        </section>


        <!-- ==================== STUDENT VIEW ==================== -->
        <section id="view-student" class="space-y-6 hidden max-w-xl mx-auto">
            <div class="bg-white p-8 rounded-2xl shadow-sm border border-slate-100 space-y-6">
                <div class="text-center space-y-2">
                    <div class="w-12 h-12 bg-emerald-50 text-emerald-600 rounded-2xl mx-auto flex items-center justify-center text-xl font-bold shadow-sm">
                        <i class="fa-solid fa-mobile-screen-button"></i>
                    </div>
                    <h2 class="text-2xl font-bold text-slate-900">โทรศัพท์นักเรียน</h2>
                    <p class="text-xs text-slate-500">เลือกห้องที่เปิดเช็คชื่อ สแกน และยืนยันตัวตน</p>
                </div>

                <!-- Step 1: Scan QR or Select Active Room -->
                <div id="student-step-1" class="space-y-4">
                    <div class="p-6 bg-slate-50 rounded-2xl border-2 border-dashed border-slate-200 text-center space-y-4">
                        <i class="fa-solid fa-camera text-3xl text-emerald-500"></i>
                        <div>
                            <p class="text-sm font-medium text-slate-700">จำลองการสแกนคิวอาร์โค้ด หรือเลือกห้องที่ครูเปิดระบบอยู่</p>
                        </div>
                        
                        <div>
                            <label class="block text-xs font-semibold text-slate-600 mb-1 text-left">ห้องเรียนที่เปิดให้เช็คชื่อขณะนี้:</label>
                            <select id="studentActiveRoomSelect" class="w-full px-4 py-3 text-sm rounded-xl border border-slate-200 focus:outline-none focus:ring-2 focus:ring-emerald-500 bg-white">
                                <option value="">-- ไม่มีห้องเรียนที่เปิดระบบ --</option>
                            </select>
                        </div>

                        <button onclick="simulateScanQR()" class="w-full bg-emerald-600 hover:bg-emerald-700 text-white font-medium px-6 py-3 rounded-xl text-sm transition-all shadow-md">
                            สแกนคิวอาร์โค้ดของครู / ดำเนินการต่อ
                        </button>
                    </div>
                </div>

                <!-- Step 2: Select Name & Confirm -->
                <div id="student-step-2" class="space-y-4 hidden">
                    <div class="bg-emerald-50 border border-emerald-200 p-4 rounded-xl flex items-center gap-3 text-emerald-800 text-sm">
                        <i class="fa-solid fa-circle-check text-lg"></i>
                        <div>
                            <p class="font-semibold" id="student-scanned-room-label">เชื่อมต่อห้องเรียนสำเร็จ!</p>
                            <p class="text-xs opacity-80">กรุณาเลือกชื่อของตัวเองเพื่อยืนยันการเข้าเรียน</p>
                        </div>
                    </div>

                    <div class="space-y-3">
                        <div>
                            <label class="block text-xs font-semibold text-slate-600 mb-1">เลือกชื่อนักเรียนของคุณ:</label>
                            <select id="studentSelectDropdown" class="w-full px-4 py-3 text-sm rounded-xl border border-slate-200 focus:outline-none focus:ring-2 focus:ring-emerald-500 bg-white">
                                <option value="">-- กรุณาเลือกชื่อของคุณ --</option>
                            </select>
                        </div>

                        <div class="pt-2">
                            <button onclick="confirmAttendance()" class="w-full bg-emerald-600 hover:bg-emerald-700 text-white font-medium py-3 rounded-xl text-sm transition-all shadow-md flex items-center justify-center gap-2">
                                <i class="fa-solid fa-paper-plane"></i> กดยืนยันเพื่อขอรหัสยืนยันการเข้าเรียน
                            </button>
                        </div>
                        <button onclick="backToStudentStep1()" class="w-full text-slate-500 text-xs py-2 hover:underline">
                            ย้อนกลับเลือกห้องใหม่
                        </button>
                    </div>
                </div>

                <!-- Step 3: Success Finish -->
                <div id="student-step-3" class="hidden text-center space-y-4 py-4">
                    <div class="w-16 h-16 bg-emerald-100 text-emerald-600 rounded-full mx-auto flex items-center justify-center text-3xl shadow-inner">
                        <i class="fa-solid fa-check"></i>
                    </div>
                    <div>
                        <h3 class="text-lg font-bold text-slate-900">เช็คชื่อสำเร็จ (เสร็จสิ้น)</h3>
                        <p class="text-xs text-slate-500 mt-1">ระบบได้บันทึกเวลาการเข้าเรียนของคุณเรียบร้อยแล้ว</p>
                    </div>
                    <div class="p-4 bg-slate-50 rounded-xl border border-slate-200 text-left text-xs space-y-1">
                        <p><strong class="text-slate-600">วิชา/ห้อง:</strong> <span id="res-room">-</span></p>
                        <p><strong class="text-slate-600">ชื่อ:</strong> <span id="res-name">-</span></p>
                        <p><strong class="text-slate-600">ชั้น:</strong> <span id="res-level">-</span></p>
                        <p><strong class="text-slate-600">เวลา:</strong> <span id="res-time">-</span></p>
                    </div>
                    <button onclick="resetStudentFlow()" class="bg-slate-200 hover:bg-slate-300 text-slate-700 font-medium px-6 py-2.5 rounded-xl text-sm transition-all w-full">
                        กลับหน้าหลักนักเรียน
                    </button>
                </div>
            </div>
        </section>

    </main>

    <footer class="bg-white border-t border-slate-100 py-4 text-center text-xs text-slate-400">
        <p>ระบบเช็คชื่อนักเรียนด้วย QR Code อิงตามแผนผังการทำงานออนไลน์ &copy; 2026</p>
    </footer>

    <!-- Custom Notification Box -->
    <div id="custom-modal" class="fixed inset-0 bg-slate-900/50 backdrop-blur-sm z-50 flex items-center justify-center hidden">
        <div class="bg-white p-6 rounded-2xl shadow-2xl max-w-sm w-full mx-4 text-center space-y-4">
            <div id="modal-icon" class="w-12 h-12 rounded-full mx-auto flex items-center justify-center text-xl"></div>
            <h3 id="modal-title" class="font-bold text-lg text-slate-900">แจ้งเตือน</h3>
            <p id="modal-message" class="text-sm text-slate-600"></p>
            <button onclick="closeModal()" class="w-full bg-blue-600 hover:bg-blue-700 text-white font-medium py-2.5 rounded-xl text-sm transition-all shadow-sm">
                ตกลง
            </button>
        </div>
    </div>

    <script>
        // Global Application State with LocalStorage persistence
        let systemRunning = false;
        let selectedActiveRoom = "";
        let classesData = JSON.parse(localStorage.getItem('qr_classes')) || ["ม.3/1 วิชาคณิตศาสตร์", "ม.3/2 วิชาวิทยาศาสตร์"];
        let studentsData = JSON.parse(localStorage.getItem('qr_students')) || [
            { name: "ด.ช. อนันต์ ใจดี", level: "ม.3/1", no: "1" },
            { name: "ด.ญ. สมหญิง รักเรียน", level: "ม.3/1", no: "2" },
            { name: "ด.ช. กิตติ มั่นคง", level: "ม.3/2", no: "1" }
        ];
        let attendanceRecords = JSON.parse(localStorage.getItem('qr_attendance')) || [];

        // Initialize UI on load
        window.onload = function() {
            renderClasses();
            renderActiveRoomDropdowns();
            renderStudentClassDropdown();
            renderAttendanceTable();
            updateTeacherUIState();
        };

        // Save data to localStorage
        function saveData() {
            localStorage.setItem('qr_classes', JSON.stringify(classesData));
            localStorage.setItem('qr_students', JSON.stringify(studentsData));
            localStorage.setItem('qr_attendance', JSON.stringify(attendanceRecords));
        }

        // Switch Roles
        function switchRole(role) {
            const teacherView = document.getElementById('view-teacher');
            const studentView = document.getElementById('view-student');
            const btnTeacher = document.getElementById('btn-teacher');
            const btnStudent = document.getElementById('btn-student');

            if (role === 'teacher') {
                teacherView.classList.remove('hidden');
                studentView.classList.add('hidden');
                btnTeacher.className = "px-5 py-2 rounded-lg font-medium text-sm transition-all bg-white text-blue-600 shadow-sm";
                btnStudent.className = "px-5 py-2 rounded-lg font-medium text-sm transition-all text-slate-600 hover:text-slate-900";
            } else {
                teacherView.classList.add('hidden');
                studentView.classList.remove('hidden');
                btnStudent.className = "px-5 py-2 rounded-lg font-medium text-sm transition-all bg-white text-blue-600 shadow-sm";
                btnTeacher.className = "px-5 py-2 rounded-lg font-medium text-sm transition-all text-slate-600 hover:text-slate-900";
                renderActiveRoomDropdowns();
            }
        }

        // Render Room Dropdowns for Teacher and Student
        function renderActiveRoomDropdowns() {
            const activeRoomSelect = document.getElementById('activeRoomSelect');
            const studentActiveRoomSelect = document.getElementById('studentActiveRoomSelect');

            let optionsHtml = '<option value="">-- กรุณาเลือกห้องเรียน --</option>';
            classesData.forEach(cls => {
                const selectedAttr = (cls === selectedActiveRoom) ? 'selected' : '';
                optionsHtml += `<option value="${cls}" ${selectedAttr}>${cls}</option>`;
            });

            activeRoomSelect.innerHTML = optionsHtml;

            // Student only sees rooms that are currently opened by teacher
            let studentOptionsHtml = '<option value="">-- ไม่มีห้องเรียนที่เปิดระบบ --</option>';
            if (systemRunning && selectedActiveRoom) {
                studentOptionsHtml = `<option value="${selectedActiveRoom}" selected>${selectedActiveRoom} (กำลังเปิดเช็คชื่อ)</option>`;
            }
            studentActiveRoomSelect.innerHTML = studentOptionsHtml;
        }

        function onActiveRoomChange() {
            const select = document.getElementById('activeRoomSelect');
            selectedActiveRoom = select.value;
            if (systemRunning) {
                generateQRCode(selectedActiveRoom);
            }
        }

        // Toggle System Status with Room Validation
        function toggleSystemStatus() {
            const roomSelect = document.getElementById('activeRoomSelect');
            selectedActiveRoom = roomSelect.value;

            if (!systemRunning) {
                if (!selectedActiveRoom) {
                    showModal("แจ้งเตือน", "กรุณาเลือกห้องเรียน/วิชาก่อนเปิดระบบสแกนเช็คชื่อ", "warning");
                    return;
                }
                systemRunning = true;
                generateQRCode(selectedActiveRoom);
                showModal("สำเร็จ", `เปิดการทำงานระบบสแกนเช็คชื่อสำหรับห้อง '${selectedActiveRoom}' เรียบร้อยแล้ว`, "success");
            } else {
                systemRunning = false;
                clearQRCode();
                showModal("สำเร็จ", "ปิดการทำงานระบบสแกนเช็คชื่อเรียบร้อยแล้ว", "warning");
            }
            updateTeacherUIState();
            renderActiveRoomDropdowns();
        }

        function updateTeacherUIState() {
            const statusText = document.getElementById('system-status-text');
            const toggleBtn = document.getElementById('toggle-sys-btn');
            const roomSelect = document.getElementById('activeRoomSelect');

            if (systemRunning) {
                statusText.innerHTML = '<span class="w-2.5 h-2.5 rounded-full bg-emerald-400 animate-pulse"></span> เปิดใช้งานอยู่';
                toggleBtn.innerText = "ปิดระบบ";
                toggleBtn.className = "bg-rose-500 hover:bg-rose-600 text-white px-4 py-2 rounded-lg text-xs font-semibold shadow-md transition-all";
                roomSelect.disabled = true;
            } else {
                statusText.innerHTML = '<span class="w-2.5 h-2.5 rounded-full bg-slate-400"></span> ปิดใช้งาน';
                toggleBtn.innerText = "เปิดระบบเช็คชื่อ";
                toggleBtn.className = "bg-emerald-500 hover:bg-emerald-600 text-white px-4 py-2 rounded-lg text-xs font-semibold shadow-md transition-all";
                roomSelect.disabled = false;
            }
        }

        // Generate working QR code using qrcode-generator library
        function generateQRCode(textData) {
            const container = document.getElementById('qrcode-container');
            const hint = document.getElementById('qr-hint');
            const badge = document.getElementById('qr-room-badge');
            
            container.innerHTML = "";
            hint.innerText = "สแกนเพื่อเช็คชื่อเข้าเรียน";
            badge.innerText = `ห้อง: ${textData}`;

            try {
                // Type number 0 means automatic calculation
                let qr = qrcode(0, 'M');
                qr.addData(`ATTENDANCE_ROOM:${textData}`);
                qr.make();
                
                // Create table element for QR representation (works across all browsers cleanly)
                let qrImgWrapper = document.createElement('div');
                qrImgWrapper.innerHTML = qr.createSvgTag({cellSize: 4, margin: 2});
                container.appendChild(qrImgWrapper);
            } catch (e) {
                container.innerHTML = '<span class="text-xs text-rose-500">เกิดข้อผิดพลาดในการสร้าง QR</span>';
            }
        }

        function clearQRCode() {
            const container = document.getElementById('qrcode-container');
            const hint = document.getElementById('qr-hint');
            const badge = document.getElementById('qr-room-badge');
            
            container.innerHTML = '<span class="text-xs text-slate-400">ยังไม่มี QR Code</span>';
            hint.innerText = "กรุณาเลือกห้องและเปิดระบบเพื่อสร้าง QR Code";
            badge.innerText = "-";
        }

        // Add Class
        function addClass(event) {
            event.preventDefault();
            const input = document.getElementById('classNameInput');
            const className = input.value.trim();
            if (className && !classesData.includes(className)) {
                classesData.push(className);
                input.value = '';
                renderClasses();
                renderActiveRoomDropdowns();
                renderStudentClassDropdown();
                saveData();
                showModal("สำเร็จ", `เพิ่มห้องเรียน '${className}' สำเร็จ`, "success");
            }
        }

        function renderClasses() {
            const container = document.getElementById('class-list');
            if (classesData.length === 0) {
                container.innerHTML = '<span class="text-xs text-slate-400 italic">ยังไม่มีห้องเรียน</span>';
                return;
            }
            container.innerHTML = classesData.map(cls => `
                <span class="bg-slate-100 text-slate-700 text-xs px-2.5 py-1 rounded-lg border border-slate-200 font-medium">${cls}</span>
            `).join('');
        }

        // Add Student
        function addStudent(event) {
            event.preventDefault();
            const name = document.getElementById('studentNameInput').value.trim();
            const level = document.getElementById('studentLevelInput').value.trim();
            const no = document.getElementById('studentNoInput').value.trim();

            if (name && level && no) {
                studentsData.push({ name, level, no });
                document.getElementById('studentNameInput').value = '';
                document.getElementById('studentLevelInput').value = '';
                document.getElementById('studentNoInput').value = '';
                saveData();
                showModal("สำเร็จ", `เพิ่มนักเรียน ${name} (${level}) สำเร็จ`, "success");
            }
        }

        function renderStudentClassDropdown() {
            const selectClass = document.getElementById('studentClassSelect');
            selectClass.innerHTML = '<option value="">เลือกห้องเรียน</option>' + classesData.map(cls => `<option value="${cls}">${cls}</option>`).join('');
        }

        function renderStudentNamesDropdown() {
            const selectStudent = document.getElementById('studentSelectDropdown');
            selectStudent.innerHTML = '<option value="">-- กรุณาเลือกชื่อของคุณ --</option>' + studentsData.map((std, index) => `<option value="${index}">${std.name} (${std.level} เลขที่ ${std.no})</option>`).join('');
        }

        // Teacher Process Finish
        function finishTeacherProcess() {
            saveData();
            showModal("เสร็จสิ้น", "บันทึกและจัดเก็บสถิติการเข้าเรียนของนักเรียนแต่ละชั้นเรียบร้อยแล้ว", "success");
        }

        // Student Flow: Simulate Scan QR
        function simulateScanQR() {
            const roomSelect = document.getElementById('studentActiveRoomSelect');
            const chosenRoom = roomSelect.value;

            if (!systemRunning || !chosenRoom) {
                showModal("แจ้งเตือน", "ยังไม่มีห้องเรียนที่เปิดระบบเช็คชื่อในขณะนี้ กรุณารอคุณครูเปิดระบบ", "warning");
                return;
            }

            document.getElementById('student-scanned-room-label').innerText = `เชื่อมต่อห้องเรียน: ${chosenRoom} สำเร็จ!`;
            renderStudentNamesDropdown();

            document.getElementById('student-step-1').classList.add('hidden');
            document.getElementById('student-step-2').classList.remove('hidden');
        }

        function backToStudentStep1() {
            document.getElementById('student-step-2').classList.add('hidden');
            document.getElementById('student-step-1').classList.remove('hidden');
        }

        // Student Flow: Confirm Attendance
        function confirmAttendance() {
            const select = document.getElementById('studentSelectDropdown');
            const roomSelect = document.getElementById('studentActiveRoomSelect');
            const index = select.value;
            const room = roomSelect.value;

            if (index === "") {
                showModal("แจ้งเตือน", "กรุณาเลือกชื่อของตัวเองก่อนกดยืนยัน", "warning");
                return;
            }

            const student = studentsData[index];
            const now = new Date();
            const timeString = now.toLocaleDateString('th-TH') + ' ' + now.toLocaleTimeString('th-TH');

            // Save record
            attendanceRecords.unshift({
                room: room,
                name: student.name,
                level: student.level,
                no: student.no,
                time: timeString
            });

            saveData();
            renderAttendanceTable();

            // Show step 3 (Finish)
            document.getElementById('student-step-2').classList.add('hidden');
            document.getElementById('student-step-3').classList.remove('hidden');
            document.getElementById('res-room').innerText = room;
            document.getElementById('res-name').innerText = student.name;
            document.getElementById('res-level').innerText = `${student.level} เลขที่ ${student.no}`;
            document.getElementById('res-time').innerText = timeString;
        }

        function resetStudentFlow() {
            document.getElementById('student-step-3').classList.add('hidden');
            document.getElementById('student-step-1').classList.remove('hidden');
            document.getElementById('studentSelectDropdown').value = "";
        }

        // Render Attendance Table
        function renderAttendanceTable() {
            const tbody = document.getElementById('attendance-table-body');
            if (attendanceRecords.length === 0) {
                tbody.innerHTML = `<tr><td colspan="5" class="py-6 text-center text-slate-400 italic">ยังไม่มีข้อมูลการเช็คชื่อในระบบ</td></tr>`;
                return;
            }

            tbody.innerHTML = attendanceRecords.map(rec => `
                <tr class="hover:bg-slate-50 transition-colors">
                    <td class="py-3 px-4 font-medium text-slate-600">${rec.no}</td>
                    <td class="py-3 px-4 font-semibold text-slate-800">${rec.name}</td>
                    <td class="py-3 px-4 text-slate-600">${rec.room || rec.level}</td>
                    <td class="py-3 px-4 text-slate-500 text-xs">${rec.time}</td>
                    <td class="py-3 px-4">
                        <span class="bg-emerald-100 text-emerald-800 text-xs font-medium px-2.5 py-1 rounded-full">มาเรียน</span>
                    </td>
                </tr>
            `).join('');
        }

        // Custom Modal Functionality
        function showModal(title, message, type = 'success') {
            const modal = document.getElementById('custom-modal');
            const titleEl = document.getElementById('modal-title');
            const msgEl = document.getElementById('modal-message');
            const iconEl = document.getElementById('modal-icon');

            titleEl.innerText = title;
            msgEl.innerText = message;

            if (type === 'success') {
                iconEl.className = "w-12 h-12 rounded-full mx-auto flex items-center justify-center text-xl bg-emerald-100 text-emerald-600";
                iconEl.innerHTML = '<i class="fa-solid fa-check"></i>';
            } else {
                iconEl.className = "w-12 h-12 rounded-full mx-auto flex items-center justify-center text-xl bg-amber-100 text-amber-600";
                iconEl.innerHTML = '<i class="fa-solid fa-triangle-exclamation"></i>';
            }

            modal.classList.remove('hidden');
        }

        function closeModal() {
            document.getElementById('custom-modal').classList.add('hidden');
        }
    </script>
</body>
</html>
