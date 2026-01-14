<html lang="ka">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Medical Directory</title>
  <style>
    :root {
        --primary-blue: #0077b6;
        --dark-blue: #023e8a;
        --light-blue: #00b4d8;
        --bg-light: #f8f9fa;
        --border-light: #e0eaf4;
        --text-color: var(--dark-blue);
        --secondary-text-color: var(--light-blue);
        --border-color: var(--primary-blue);
        --error-red: #dc3545;
        --success-green: #28a745;
        --white: #ffffff;
        --shadow-sm: 0 2px 8px rgba(0,0,0,0.08);
        --shadow-md: 0 4px 16px rgba(0,0,0,0.12);
        --shadow-lg: 0 8px 32px rgba(0,119,182,0.2);
    }

    * { box-sizing: border-box; margin: 0; padding: 0; }

    body {
      font-family: 'Inter', -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;
      background: linear-gradient(135deg, #f5f7fa 0%, #e8f0f7 100%);
      color: var(--text-color);
      min-height: 100vh;
      line-height: 1.6;
      display: flex;
      flex-direction: column;
    }

    /* Firebase Connection Status */
    .firebase-status {
      position: fixed;
      top: 20px;
      right: 20px;
      padding: 12px 20px;
      border-radius: 30px;
      font-size: 14px;
      font-weight: 600;
      box-shadow: var(--shadow-md);
      display: flex;
      align-items: center;
      gap: 8px;
      z-index: 10;
      transition: all 0.3s ease;
    }

    .firebase-status.connected { background: var(--success-green); color: white; }
    .firebase-status.disconnected { background: var(--error-red); color: white; }
    .firebase-status.connecting { background: #ffc107; color: #333; }

    .status-dot {
      width: 10px;
      height: 10px;
      border-radius: 50%;
      background: white;
      animation: pulse 2s infinite;
    }

    @keyframes pulse {
      0%, 100% { opacity: 1; }
      50% { opacity: 0.5; }
    }

    .container {
      max-width: 1400px;
      margin: 0 auto;
      padding: 40px 32px;
      flex: 1;
    }

    header {
      text-align: center;
      margin-bottom: 48px;
      background: var(--white);
      padding: 32px 24px;
      border-radius: 16px;
      box-shadow: var(--shadow-sm);
      border-left: 4px solid var(--primary-blue);
    }

    h1 {
      font-size: 32px;
      font-weight: 800;
      margin: 0 0 8px;
      letter-spacing: -0.5px;
      color: var(--dark-blue);
      line-height: 1.3;
    }

    .subtitle {
      font-size: 18px;
      color: var(--light-blue);
      margin: 0;
      font-weight: 600;
    }

    .controls-wrapper {
      background: var(--white);
      padding: 24px;
      border-radius: 16px;
      box-shadow: var(--shadow-sm);
      margin-bottom: 32px;
    }

    .controls {
      display: grid;
      grid-template-columns: 2fr 1.5fr auto;
      gap: 16px;
      margin-bottom: 20px;
      align-items: center;
    }

    .search-box, .filter-box {
      position: relative;
      z-index: 1;
    }

    .search-box input, .filter-box select {
      width: 100%;
      padding: 14px 18px;
      border: 2px solid var(--border-light);
      border-radius: 10px;
      font-size: 15px;
      font-family: inherit;
      background: var(--white);
      color: var(--text-color);
      transition: all 0.2s ease;
      font-weight: 500;
    }

    .search-box input:focus, .filter-box select:focus {
      outline: none;
      border-color: var(--primary-blue);
      box-shadow: 0 0 0 4px rgba(0, 119, 182, 0.1);
    }

    .filter-box select { cursor: pointer; }

    .action-buttons { display: flex; gap: 12px; }

    button {
      padding: 14px 24px;
      border: 2px solid var(--primary-blue);
      background: var(--primary-blue);
      color: var(--white);
      font-size: 15px;
      font-weight: 600;
      border-radius: 10px;
      cursor: pointer;
      font-family: inherit;
      transition: all 0.2s ease;
      box-shadow: var(--shadow-sm);
      white-space: nowrap;
    }

    button:hover {
      background: var(--dark-blue);
      border-color: var(--dark-blue);
      transform: translateY(-2px);
      box-shadow: var(--shadow-md);
    }

    button:active { transform: translateY(0); }

    #print-btn, #add-doctor-btn {
      background: var(--white);
      color: var(--primary-blue);
    }

    #print-btn:hover, #add-doctor-btn:hover {
      background: var(--border-light);
      color: var(--dark-blue);
      border-color: var(--dark-blue);
    }

    #add-doctor-btn {
      border-color: var(--success-green);
      color: var(--success-green);
      font-weight: 700;
    }

    #add-doctor-btn:hover {
      background: var(--success-green);
      color: var(--white);
      border-color: var(--success-green);
    }

    button:disabled {
      opacity: 0.5;
      cursor: not-allowed;
      transform: none;
      box-shadow: none;
    }

    .sort-controls { display: flex; gap: 10px; }

    .sort-btn {
      padding: 10px 20px;
      font-size: 14px;
      border: 2px solid var(--border-light);
      background: var(--white);
      color: var(--primary-blue);
      transition: all 0.2s ease;
      font-weight: 600;
    }

    .sort-btn:hover { background: var(--border-light); transform: translateY(0); }
    .sort-btn.active { background: var(--primary-blue); color: var(--white); border-color: var(--primary-blue); }
    .sort-btn.active:hover { background: var(--dark-blue); }

    .loading {
      text-align: center;
      padding: 60px;
      font-size: 18px;
      color: var(--primary-blue);
      font-weight: 600;
    }

    .spinner {
      display: inline-block;
      width: 28px;
      height: 28px;
      border: 3px solid rgba(0,119,182,0.2);
      border-top: 3px solid var(--primary-blue);
      border-radius: 50%;
      animation: spin 0.8s linear infinite;
      margin-right: 12px;
      vertical-align: middle;
    }

    @keyframes spin {
      0% { transform: rotate(0deg); }
      100% { transform: rotate(360deg); }
    }

    .doctors-list {
      display: grid;
      grid-template-columns: repeat(auto-fill, minmax(320px, 1fr));
      gap: 24px;
    }

    .no-results {
      text-align: center;
      padding: 80px 20px;
      font-size: 18px;
      color: var(--light-blue);
      font-weight: 600;
      background: var(--white);
      border-radius: 16px;
      box-shadow: var(--shadow-sm);
    }

    .doctor-card {
      border: 2px solid var(--border-light);
      padding: 28px;
      border-radius: 14px;
      background: var(--white);
      box-shadow: var(--shadow-sm);
      transition: all 0.3s cubic-bezier(0.25,0.8,0.25,1);
      position: relative;
      overflow: hidden;
    }

    .doctor-card::before {
      content: '';
      position: absolute;
      top: 0;
      left: 0;
      width: 4px;
      height: 100%;
      background: var(--primary-blue);
      transform: scaleY(0);
      transition: transform 0.3s ease;
    }

    .doctor-card:hover::before { transform: scaleY(1); }

    .doctor-card:hover {
      box-shadow: var(--shadow-lg);
      transform: translateY(-4px);
      border-color: var(--primary-blue);
    }

    .doctor-name {
      font-size: 22px;
      font-weight: 700;
      margin: 0 0 8px;
      color: var(--dark-blue);
      letter-spacing: -0.3px;
    }

    .doctor-specialty {
      font-size: 15px;
      color: var(--light-blue);
      margin: 0 0 20px;
      font-weight: 600;
      padding: 6px 12px;
      background: rgba(0, 180, 216, 0.1);
      border-radius: 6px;
      display: inline-block;
    }

    .doctor-phone { font-size: 17px; font-weight: 600; }

    .doctor-phone a {
      color: var(--primary-blue);
      text-decoration: none;
      border-bottom: 2px solid transparent;
      transition: all 0.2s ease;
      padding-bottom: 2px;
    }

    .doctor-phone a:hover {
      color: var(--dark-blue);
      border-bottom-color: var(--dark-blue);
    }

    /* Modal Styles */
    .modal {
      display: none;
      position: fixed;
      z-index: 1000;
      left: 0;
      top: 0;
      width: 100%;
      height: 100%;
      background-color: rgba(0, 0, 0, 0.6);
      backdrop-filter: blur(4px);
      animation: fadeIn 0.2s ease;
    }

    @keyframes fadeIn { from { opacity: 0; } to { opacity: 1; } }
    @keyframes slideUp {
      from { opacity: 0; transform: translate(-50%, -45%); }
      to { opacity: 1; transform: translate(-50%, -50%); }
    }

    .modal-content {
      background-color: var(--white);
      position: absolute;
      left: 50%;
      top: 50%;
      transform: translate(-50%, -50%);
      padding: 40px;
      border-radius: 20px;
      box-shadow: 0 20px 60px rgba(0, 0, 0, 0.3);
      max-width: 500px;
      width: 90%;
      animation: slideUp 0.3s ease;
      z-index: 1001;
    }

    .modal-header {
      margin-bottom: 28px;
      padding-bottom: 20px;
      border-bottom: 2px solid var(--border-light);
    }

    .modal-header h2 {
      font-size: 26px;
      font-weight: 800;
      color: var(--dark-blue);
      margin: 0;
    }

    .close {
      color: #aaa;
      float: right;
      font-size: 32px;
      font-weight: bold;
      line-height: 1;
      cursor: pointer;
      transition: color 0.2s;
    }

    .close:hover, .close:focus { color: var(--error-red); }

    .form-group { margin-bottom: 24px; }

    .form-group label {
      display: block;
      margin-bottom: 8px;
      font-weight: 600;
      color: var(--dark-blue);
      font-size: 14px;
      text-transform: uppercase;
      letter-spacing: 0.5px;
    }

    .form-group input, .form-group select {
      width: 100%;
      padding: 14px 16px;
      border: 2px solid var(--border-light);
      border-radius: 10px;
      font-size: 15px;
      font-family: inherit;
      transition: all 0.2s ease;
      font-weight: 500;
    }

    .form-group input:focus, .form-group select:focus {
      outline: none;
      border-color: var(--primary-blue);
      box-shadow: 0 0 0 4px rgba(0, 119, 182, 0.1);
    }

    .modal-buttons {
      display: flex;
      gap: 12px;
      margin-top: 32px;
    }

    .modal-buttons button {
      flex: 1;
      padding: 16px;
      font-size: 16px;
      font-weight: 700;
    }

    .btn-cancel {
      background: var(--white);
      color: var(--text-color);
      border-color: var(--border-light);
    }

    .btn-cancel:hover {
      background: var(--border-light);
      border-color: var(--border-light);
    }

    .error-message {
      color: var(--error-red);
      font-size: 13px;
      margin-top: 6px;
      display: none;
      font-weight: 600;
    }

    .success-message {
      background: var(--success-green);
      color: white;
      padding: 12px 20px;
      border-radius: 8px;
      margin-bottom: 20px;
      display: none;
      font-weight: 600;
      text-align: center;
    }

    .footer {
      margin-top: auto;
      text-align: center;
      padding: 24px 0;
      font-size: 14px;
      color: #6c757d;
      background: transparent;
      font-weight: 600;
    }

    .footer p { margin: 0; }

    @media print {
      .controls-wrapper, .action-buttons, .sort-controls, button, .footer, .modal, .firebase-status {
        display: none !important;
      }
      .container { max-width: 100%; padding: 0; }
      .doctor-card {
        break-inside: avoid;
        box-shadow: none !important;
        border: 1px solid #ccc !important;
        margin-bottom: 15px;
      }
      body { background: #ffffff; }
      header { box-shadow: none; border: 1px solid #ccc; }
    }

    @media (max-width: 968px) {
      .controls { grid-template-columns: 1fr; }
      .action-buttons { width: 100%; }
      .action-buttons button { flex: 1; }
      .firebase-status {
        top: 10px;
        right: 10px;
        font-size: 12px;
        padding: 8px 14px;
      }
    }

    @media (max-width: 768px) {
      .container { padding: 24px 16px; }
      h1 { font-size: 24px; }
      .subtitle { font-size: 16px; }
      header { padding: 24px 20px; }
      .controls-wrapper { padding: 20px; }
      .sort-controls { flex-wrap: wrap; gap: 8px; }
      .doctors-list { grid-template-columns: 1fr; }
      .modal-content { padding: 28px 24px; }
    }
  </style>
</head>

<body>
  <!-- Firebase Connection Status -->
  <div id="firebase-status" class="firebase-status connecting">
    <span class="status-dot"></span>
    <span id="status-text">უკავშირდება Firebase-ს...</span>
  </div>

  <div id="main-content">
    <div class="container">
      <header>
        <h1 id="clinic-title">თბილისის სახელმწიფო სამედიცინო უნივერსიტეტისა და ინგოროყვას მაღალი სამედიცინო ტექნოლოგიების საუნივერსიტეტო კლინიკა</h1>
        <p class="subtitle" id="clinic-subtitle">ექიმების სატელეფონო სია</p>
      </header>

      <div class="success-message" id="success-message"></div>

      <div class="controls-wrapper">
        <div class="controls">
          <div class="search-box">
            <input type="text" id="search-input" placeholder="ძებნა სახელით, გვარით ან ტელეფონის ნომრით..." aria-label="Search doctors">
          </div>
          <div class="filter-box">
            <select id="specialty-filter" aria-label="Filter by specialty">
              <option value="">ყველა სპეციალობა</option>
            </select>
          </div>
          <div class="action-buttons">
            <button id="add-doctor-btn" type="button">+ დამატება</button>
            <button id="refresh-btn" type="button"><span id="refresh-text">განახლება</span></button>
            <button id="print-btn" type="button" onclick="window.print()"><span id="print-text">ბეჭდვა</span></button>
          </div>
        </div>

        <div class="sort-controls">
          <button class="sort-btn active" id="sort-name" type="button">A to Z (სახელი)</button>
          <button class="sort-btn" id="sort-specialty" type="button">სპეციალობა</button>
        </div>
      </div>

      <div id="loading" class="loading" style="display: none;">
        <span class="spinner"></span> მონაცემები იტვირთება...
      </div>

      <div id="doctors-list" class="doctors-list"></div>

      <div id="no-results" class="no-results" style="display: none;">
        შედეგები არ მოიძებნა
      </div>
    </div>
  </div>

  <!-- Add Doctor Modal -->
  <div id="add-doctor-modal" class="modal">
    <div class="modal-content">
      <div class="modal-header">
        <span class="close">&times;</span>
        <h2>ახალი ექიმის დამატება</h2>
      </div>

      <form id="add-doctor-form">
        <div class="form-group">
          <label for="doctor-name">სახელი და გვარი *</label>
          <input type="text" id="doctor-name" required placeholder="მაგ: გიორგი გელაშვილი">
          <div class="error-message" id="name-error">გთხოვთ შეიყვანოთ სახელი და გვარი</div>
        </div>

        <div class="form-group">
          <label for="doctor-specialty">განყოფილება *</label>
          <select id="doctor-specialty" required>
            <option value="">აირჩიეთ განყოფილება</option>
            <option>CT ოპერატორი</option>
            <option>CT რადიოლოგი</option>
            <option>ანგიოქირურგია</option>
            <option>ანესთეზია</option>
            <option>ბავშვთა გადაუდებელი მედიცინა</option>
            <option>ბავშვთა ონკო-ჰემატოლოგია</option>
            <option>ბავშვთა ქირურგია</option>
            <option>ბავშვთა რეანიმაცია</option>
            <option>გადაუდებელი მედიცინა</option>
            <option>გინეკოლოგია</option>
            <option>ენდოსკოპია</option>
            <option>ექოსკოპია</option>
            <option>ზოგადი რეანიმაცია</option>
            <option>ზოგადი ქირურგია</option>
            <option>თორაკო ქირურგია</option>
            <option>ინფექციური სნეულებები</option>
            <option>კარდიო რეანიმაცია</option>
            <option>კარდიოლოგია</option>
            <option>ლაბორატორია</option>
            <option>ნევროლოგია</option>
            <option>ნეირო ქირურგია</option>
            <option>ნეირო რეანიმაცია</option>
            <option>ნეფროლოგია</option>
            <option>პედიატრია</option>
            <option>პულმონოლოგია</option>
            <option>X-ray რადიოლოგი</option>
            <option>რენტგენი</option>
            <option>ტრავმატოლოგია</option>
            <option>უროლოგია</option>
            <option>ყბა–სახის ქირურგია</option>
            <option>შინაგანი მედიცინა</option>
            <option>ჰემატოლოგია 3</option>
            <option>ჰემატოლოგია 10</option>
            <option>ჰეპატოლოგია</option>
          </select>
          <div class="error-message" id="specialty-error">გთხოვთ აირჩიოთ განყოფილება</div>
        </div>

        <div class="form-group">
          <label for="doctor-phone">ტელეფონის ნომერი *</label>
          <input type="tel" id="doctor-phone" required placeholder="მაგ: 599 12 34 56">
          <div class="error-message" id="phone-error">გთხოვთ შეიყვანოთ ტელეფონის ნომერი</div>
        </div>

        <div class="modal-buttons">
          <button type="button" class="btn-cancel" id="cancel-btn">გაუქმება</button>
          <button type="submit" id="submit-btn">დამატება</button>
        </div>
      </form>
    </div>
  </div>

  <footer class="footer">
    <p>created by IMED🩺</p>
  </footer>

  <!-- Firebase SDK v9 (Modular) -->
  <script type="module">
    import { initializeApp } from 'https://www.gstatic.com/firebasejs/10.7.1/firebase-app.js';
    import {
      getFirestore,
      collection,
      addDoc,
      onSnapshot,
      serverTimestamp
    } from 'https://www.gstatic.com/firebasejs/10.7.1/firebase-firestore.js';

    // ============ UI HELPERS ============
    function updateFirebaseStatus(status, detailText = '') {
      const statusElement = document.getElementById('firebase-status');
      const statusText = document.getElementById('status-text');
      statusElement.className = 'firebase-status ' + status;

      if (status === 'connected') {
        statusText.textContent = '✓ Firebase დაკავშირებულია';
      } else if (status === 'disconnected') {
        statusText.textContent = detailText ? ('✗ ' + detailText) : '✗ Firebase გამორთულია';
      } else {
        statusText.textContent = 'უკავშირდება...';
      }
    }

    function setLoading(on) {
      const loading = document.getElementById('loading');
      const list = document.getElementById('doctors-list');
      const nores = document.getElementById('no-results');

      loading.style.display = on ? 'block' : 'none';
      if (on) {
        list.style.display = 'none';
        nores.style.display = 'none';
      }
    }

    // Show success/error message
    function showMessage(message, type) {
      const messageEl = document.getElementById('success-message');
      messageEl.textContent = message;
      messageEl.style.display = 'block';
      messageEl.style.background = type === 'success' ? 'var(--success-green)' : 'var(--error-red)';
      setTimeout(() => { messageEl.style.display = 'none'; }, 3500);
    }

    function normalizePhone(s) {
      return String(s || '').replace(/[^0-9]/g, '');
    }

    // ============ FIREBASE CONFIG ============
    const firebaseConfig = {
      apiKey: "AIzaSyAjGwo4BWRR2BZNewA91oiTSJjMePXNenE",
      authDomain: "medical-phone-directory.firebaseapp.com",
      projectId: "medical-phone-directory",
      storageBucket: "medical-phone-directory.firebasestorage.app",
      messagingSenderId: "550121336289",
      appId: "1:550121336289:web:0b9d9661886136efebb6a0",
      measurementId: "G-6N279Y263K"
    };

    let app, db;
    let isFirebaseConnected = false;

    try {
      app = initializeApp(firebaseConfig);
      db = getFirestore(app);
      isFirebaseConnected = true;
      updateFirebaseStatus('connected');
      console.log('✅ Firebase დაკავშირებულია წარმატებით!');
    } catch (error) {
      isFirebaseConnected = false;
      updateFirebaseStatus('disconnected', 'Firebase-თან კავშირი ვერ მოხერხდა');
      console.error('❌ Firebase-თან დაკავშირება ვერ მოხერხდა:', error);
    }

    let unsubscribe = null;
    let allDoctors = [];
    let currentSort = 'name';

    // Initial Doctors List - მუდმივი სია
    const initialDoctors = [
      { name: 'პაატა ბარათაშვილი', specialty: 'X-ray რადიოლოგი', phone: '593 311 748', isInitial: true },
      { name: 'ვაჟა თავბერიძე', specialty: 'X-ray რადიოლოგი', phone: '551 470 471', isInitial: true },
      { name: 'მაია', specialty: 'რენტგენი', phone: '557 654 351', isInitial: true },
      { name: 'ნინო', specialty: 'რენტგენი', phone: '599 400 311', isInitial: true },
      { name: 'ნაზი', specialty: 'რენტგენი', phone: '555 181 801', isInitial: true },
      { name: 'მარიამი', specialty: 'რენტგენი', phone: '598 100 644', isInitial: true },
      { name: 'ნიკა მაჩაიძე', specialty: 'CT ოპერატორი', phone: '598 295 798', isInitial: true },
      { name: 'მარიამი', specialty: 'CT ოპერატორი', phone: '599 216 624', isInitial: true },
      { name: 'ზურა ქოჩერაშვილი', specialty: 'CT ოპერატორი', phone: '557 767 362', isInitial: true },
      { name: 'მაია დემურიშვილი', specialty: 'CT რადიოლოგი', phone: '555 258 800', isInitial: true },
      { name: 'ვალერიანე უხურგუნაშვილი', specialty: 'CT რადიოლოგი', phone: '558 333 455', isInitial: true },
      { name: 'ცისია კახაძე', specialty: 'CT რადიოლოგი', phone: '599 407 560', isInitial: true },
      { name: 'ჯუბა ნაზარაშვილი', specialty: 'CT ოპერატორი', phone: '571 036 317', isInitial: true },
      { name: 'მანანა გოგოლაძე', specialty: 'ექოსკოპია', phone: '577 450 049', isInitial: true },
      { name: 'ანა ინგოროყვა', specialty: 'ექოსკოპია', phone: '599 222 201', isInitial: true },
      { name: 'მარიამ გავაშელი', specialty: 'ექოსკოპია', phone: '544 447 346', isInitial: true },
      { name: 'თამარ გოგელია', specialty: 'ექოსკოპია', phone: '557 424 363', isInitial: true },
      { name: 'ირინა მოდებაძე', specialty: 'ექოსკოპია', phone: '577 090 967', isInitial: true },
      { name: 'ლაბორატორია', specialty: 'ლაბორატორია', phone: '577 101 949', isInitial: true },
      { name: 'ირაკლი დევიძე', specialty: 'ყბა–სახის ქირურგია', phone: '597 03 05 40', isInitial: true },
      { name: 'გიორგი გვენეტაძე', specialty: 'ყბა–სახის ქირურგია', phone: '599 62 99 91', isInitial: true },
      { name: 'ერეკლე გელაშვილი', specialty: 'ყბა–სახის ქირურგია', phone: '597 02 20 99', isInitial: true },
      { name: 'ნუნუკა გურაბანიძე', specialty: 'ყბა–სახის ქირურგია', phone: '551 159 797', isInitial: true },
      { name: 'გრიგოლ ჯავახაძე', specialty: 'ყბა–სახის ქირურგია', phone: '597 098 116', isInitial: true },
      { name: 'შალვა ჭოველიძე', specialty: 'უროლოგია', phone: '577 460 025', isInitial: true },
      { name: 'ნიკოლოზ გვარამია', specialty: 'უროლოგია', phone: '597 774 091', isInitial: true },
      { name: 'ვუგარ სადიკოვი', specialty: 'უროლოგია', phone: '557 175 005', isInitial: true },
      { name: 'ნანა გოგოხია', specialty: 'უროლოგია', phone: '557 497 474', isInitial: true },
      { name: 'მარიკა ყურაშვილი', specialty: 'უროლოგია', phone: '555 213 650', isInitial: true },
      { name: 'ზაური თაქთაქიშილი', specialty: 'უროლოგია', phone: '551 591 774', isInitial: true },
      { name: 'გიგი ორაგველიძე', specialty: 'უროლოგია', phone: '511 282 879', isInitial: true },
      { name: 'გიორგი ხიზანიშვილი', specialty: 'ტრავმატოლოგია', phone: '595 914 096', isInitial: true },
      { name: 'კახა გოშაძე', specialty: 'ტრავმატოლოგია', phone: '598 787 859', isInitial: true },
      { name: 'ზურა ჩხარტიშვილი', specialty: 'ტრავმატოლოგია', phone: '599 055 181', isInitial: true },
      { name: 'ნიკა ლომიძე', specialty: 'ტრავმატოლოგია', phone: '599 808 191', isInitial: true },
      { name: 'ნიკა რაზმაძე', specialty: 'ტრავმატოლოგია', phone: '579 775 674', isInitial: true },
      { name: 'გურამ ჩაჩუა', specialty: 'ნეირო ქირურგია', phone: '579 031 178', isInitial: true },
      { name: 'მიხეილ გურასპიშვილი', specialty: 'ნეირო ქირურგია', phone: '555 191 378', isInitial: true },
      { name: 'ოთარ გახოკია', specialty: 'ნეირო ქირურგია', phone: '558 344 233', isInitial: true },
      { name: 'არჩილ წიკლაური', specialty: 'ნეირო ქირურგია', phone: '558 566 848', isInitial: true },
      { name: 'ლუკა ლეკაშვილი', specialty: 'ნეირო ქირურგია', phone: '595 455 135', isInitial: true },
      { name: 'ლუკა გოგოტიშვილი', specialty: 'ნეირო ქირურგია', phone: '592 861 741', isInitial: true },
      { name: 'კორპორატიული', specialty: 'ნეირო ქირურგია', phone: '511 453 571', isInitial: true },
      { name: 'ნეირორეანიმაცია', specialty: 'ნეირო რეანიმაცია', phone: '511 453 576', isInitial: true },
      { name: 'ნინო ხარაიშვილი', specialty: 'ნევროლოგია', phone: '593 151 588', isInitial: true },
      { name: 'ნათია ხაჩიძე', specialty: 'ნევროლოგია', phone: '598 61 06 24', isInitial: true },
      { name: 'ალექსი მაღლაკელიძე', specialty: 'ნევროლოგია', phone: '591 06 52 37', isInitial: true },
      { name: 'თამთა კარანაძე', specialty: 'ნევროლოგია', phone: '577 395 080', isInitial: true },
      { name: 'ჟანა', specialty: 'ნევროლოგია', phone: '579 379 252', isInitial: true },
      { name: 'ქრისტინე დვალაძე', specialty: 'ნევროლოგია', phone: '568 03 03 36', isInitial: true },
      { name: 'ნათია კურტანიძე', specialty: 'ნევროლოგია', phone: '599 70 57 33', isInitial: true },
      { name: 'ანა შუბითიძე', specialty: 'ნევროლოგია', phone: '555 37 59 68', isInitial: true },
      { name: 'ანა ქურხული', specialty: 'ნევროლოგია', phone: '568 908 466', isInitial: true },
      { name: 'ირინა ჯაჯანიძე', specialty: 'გინეკოლოგია', phone: '599 90 14 58', isInitial: true },
      { name: 'რუსუდან ფიცხელაური', specialty: 'გინეკოლოგია', phone: '599 67 61 40', isInitial: true },
      { name: 'ნინო ხათრიძე', specialty: 'გინეკოლოგია', phone: '598 48 21 42', isInitial: true },
      { name: 'დიანა მირზაშვილი', specialty: 'გინეკოლოგია', phone: '599 90 42 98', isInitial: true },
      { name: 'თინა ჩალიგავა', specialty: 'გინეკოლოგია', phone: '599 13 07 08', isInitial: true },
      { name: 'ნინო შარაშენიძე', specialty: 'ჰემატოლოგია 3', phone: '599 91 49 91', isInitial: true },
      { name: 'ია მალაშხია', specialty: 'ჰემატოლოგია 3', phone: '599 490 305', isInitial: true },
      { name: 'შამო მუსაევი', specialty: 'ჰემატოლოგია 10', phone: '557 949 226', isInitial: true },
      { name: 'თაკო აზიკური', specialty: 'ჰემატოლოგია 10', phone: '593 545 233', isInitial: true },
      { name: 'ნატალია ნადიკაშვილი', specialty: 'ჰემატოლოგია 10', phone: '577 222 970', isInitial: true },
      { name: 'ჯონდი ჭავჭანიძე', specialty: 'ენდოსკოპია', phone: '577 453 405', isInitial: true },
      { name: 'დავით გობეჯიშვილი', specialty: 'ენდოსკოპია', phone: '599 933 584', isInitial: true },
      { name: 'თეიმურაზ სამადაშვილი', specialty: 'ენდოსკოპია', phone: '598 22 22 46', isInitial: true },
      { name: 'ირაკლი შეკლაშვილი', specialty: 'ენდოსკოპია', phone: '577 339 956', isInitial: true },
      { name: 'მარიკა წერეთელი', specialty: 'ინფექციური სნეულებები', phone: '593 362 987', isInitial: true },
      { name: 'ნუცა დონაძე', specialty: 'ინფექციური სნეულებები', phone: '599 89 08 29', isInitial: true },
      { name: 'თაკო ზაზაძე', specialty: 'ინფექციური სნეულებები', phone: '597 777 113', isInitial: true },
      { name: 'თამარ წერეთელი', specialty: 'ინფექციური სნეულებები', phone: '555 558 333', isInitial: true },
      { name: 'ია ბაღაშვილი', specialty: 'ინფექციური სნეულებები', phone: '577 58 82 05', isInitial: true },
      { name: 'ირმა მარკოიძე', specialty: 'ინფექციური სნეულებები', phone: '599 470 228', isInitial: true },
      { name: 'ციცი მაღლაფერიძე', specialty: 'ინფექციური სნეულებები', phone: '579 70 60 81', isInitial: true },
      { name: 'ნინო წურწუმია', specialty: 'ინფექციური სნეულებები', phone: '557 58 78 34', isInitial: true },
      { name: 'ნინო აბულაძე', specialty: 'ინფექციური სნეულებები', phone: '599 060 194', isInitial: true },
      { name: 'თაკო ზაზაძე', specialty: 'ინფექციური სნეულებები', phone: '598 005 186', isInitial: true },
      { name: 'ირინა კილაძე', specialty: 'ინფექციური სნეულებები', phone: '599 88 35 77', isInitial: true },
      { name: 'ეკატერინე მარკოზია', specialty: 'ინფექციური სნეულებები', phone: '555 739 633', isInitial: true },
      { name: 'ლაშა სარალიღე', specialty: 'ზოგადი ქირურგია', phone: '599 977 762', isInitial: true },
      { name: 'მაია ლობჟანიძე', specialty: 'ზოგადი ქირურგია', phone: '577 671 710', isInitial: true },
      { name: 'დავით ვარდოსანიძე', specialty: 'ზოგადი ქირურგია', phone: '577 671 705', isInitial: true },
      { name: 'ზაზა მანელიძე', specialty: 'ზოგადი ქირურგია', phone: '595 582 876', isInitial: true },
      { name: 'ირაკლი კაჭახიძე', specialty: 'ზოგადი ქირურგია', phone: '577 671 707', isInitial: true },
      { name: 'ლალი ახმეტელი', specialty: 'ზოგადი ქირურგია', phone: '577 553 311', isInitial: true },
      { name: 'ლია საგინაშვილი', specialty: 'ზოგადი ქირურგია', phone: '599 503 567', isInitial: true },
      { name: 'ბესო ირემაშვილი', specialty: 'ზოგადი ქირურგია', phone: '595 300 719', isInitial: true },
      { name: 'ონისე ტყეშელაშვილი', specialty: 'ზოგადი ქირურგია', phone: '574 219 219', isInitial: true },
      { name: 'გიორგი შუბითიძე', specialty: 'ზოგადი ქირურგია', phone: '595 418 040', isInitial: true },
      { name: 'გუგა ზაალიშვილი', specialty: 'თორაკო ქირურგია', phone: '577 459 556', isInitial: true },
      { name: 'რობერტი გობეჩია', specialty: 'თორაკო ქირურგია', phone: '599 931 220', isInitial: true },
      { name: 'ვასო ბაბიაშვილი', specialty: 'თორაკო ქირურგია', phone: '557 752 565', isInitial: true },
      { name: 'დათო მარკოზია', specialty: 'თორაკო ქირურგია', phone: '593 100 176', isInitial: true },
      { name: 'ლევან ქაცარავა', specialty: 'თორაკო ქირურგია', phone: '593 696 743', isInitial: true },
      { name: 'ირინა სვიანაძე', specialty: 'პულმონოლოგია', phone: '555 539 733', isInitial: true },
      { name: 'ლანა ბერია', specialty: 'პულმონოლოგია', phone: '598 358 377', isInitial: true },
      { name: 'ნინო ღრუბელაშვილი', specialty: 'ზოგადი რეანიმაცია', phone: '599 943 008', isInitial: true },
      { name: 'ასიკო ენუქიძე', specialty: 'ზოგადი რეანიმაცია', phone: '577 101 910', isInitial: true },
      { name: 'თიკო კუჭავა', specialty: 'ზოგადი რეანიმაცია', phone: '599 425 646', isInitial: true },
      { name: 'დათო კახიძე', specialty: 'ზოგადი რეანიმაცია', phone: '598 535 337', isInitial: true },
      { name: 'შორენა მურმანიშვილი', specialty: 'ზოგადი რეანიმაცია', phone: '599 361 288', isInitial: true },
      { name: 'თამუნა ხუციშვილი', specialty: 'ზოგადი რეანიმაცია', phone: '599 141 380', isInitial: true },
      { name: 'ლიკა ქობლიანიძე', specialty: 'ზოგადი რეანიმაცია', phone: '599 313 345', isInitial: true },
      { name: 'ნათია ჯიყაშვილი', specialty: 'ზოგადი რეანიმაცია', phone: '592 058 180', isInitial: true },
      { name: 'ვახტანგ ჩიქოვანი', specialty: 'ზოგადი რეანიმაცია', phone: '599 420 576', isInitial: true },
      { name: 'მარიამ მერებაშვილი', specialty: 'ზოგადი რეანიმაცია', phone: '598 477 662', isInitial: true },
      { name: 'დათო მამინაშვილი', specialty: 'შინაგანი მედიცინა', phone: '579 49 15 51', isInitial: true },
      { name: 'ნიკო გოგალაძე', specialty: 'შინაგანი მედიცინა', phone: '568 96 13 85', isInitial: true },
      { name: 'გვანცა ხაჩიაშვილი', specialty: 'შინაგანი მედიცინა', phone: '571 187 920', isInitial: true },
      { name: 'ნათია ეფრემიძე', specialty: 'შინაგანი მედიცინა', phone: '557 752 842', isInitial: true },
      { name: 'ნინო მიტიჩაშვილი', specialty: 'ჰეპატოლოგია', phone: '579 559 558', isInitial: true },
      { name: 'მარიამ ბერიძე', specialty: 'ნეფროლოგია', phone: '599 758 607', isInitial: true },
      { name: 'მაკა ტაბაღუა', specialty: 'ნეფროლოგია', phone: '598 77 88 12', isInitial: true },
      { name: 'მარიამ გიუაშვილი', specialty: 'ნეფროლოგია', phone: '551 45 21 21', isInitial: true },
      { name: 'რუსუდან რუსია', specialty: 'ნეფროლოგია', phone: '599 11 15 11', isInitial: true },
      { name: 'ნორა სარიშვილი', specialty: 'ნეფროლოგია', phone: '593 128 485', isInitial: true },
      { name: 'სალომე დარასელია', specialty: 'ნეფროლოგია', phone: '574 54 37 37', isInitial: true },
      { name: 'გიორგი გაზდელიანი', specialty: 'ნეფროლოგია', phone: '591 000 604', isInitial: true },
      { name: 'ანა ჭიქაბერიძე', specialty: 'ნეფროლოგია', phone: '599 103 106', isInitial: true },
      { name: 'ხაიალ დემურჩიევ', specialty: 'ნეფროლოგია', phone: '577 591 644', isInitial: true },
      { name: 'ნინო ბუაძე', specialty: 'ნეფროლოგია', phone: '593 494 995', isInitial: true },
      { name: 'თეონა ხელაშვილი', specialty: 'ნეფროლოგია', phone: '557 438 626', isInitial: true },
      { name: 'გვანცა მეცხვარიშვილი', specialty: 'ნეფროლოგია', phone: '597 777 991', isInitial: true },
      { name: 'თამარ კასრაძე', specialty: 'ნეფროლოგია', phone: '593 329 900', isInitial: true },
      { name: 'ნონა ბაბუციძე', specialty: 'ნეფროლოგია', phone: '555 595 550', isInitial: true },
      { name: 'თამარ თევდორაძე', specialty: 'ნეფროლოგია', phone: '551 770 505', isInitial: true },
      { name: 'ქეთევან დალაქიშვილი', specialty: 'ნეფროლოგია', phone: '599 194 353', isInitial: true },
      { name: 'თამარ ბაგაშვილი', specialty: 'ნეფროლოგია', phone: '593 934 241', isInitial: true },
      { name: 'ქეთი კაპანაძე', specialty: 'ნეფროლოგია', phone: '598 232 177', isInitial: true },
      { name: 'ზურაბ გოგინაშვილი', specialty: 'ანგიოქირურგია', phone: '599 11 34 57', isInitial: true },
      { name: 'ზურაბ გოგიჩაშვილი', specialty: 'ანგიოქირურგია', phone: '599 55 80 60', isInitial: true },
      { name: 'დათო ბაბილოძე', specialty: 'ანგიოქირურგია', phone: '599 520 938', isInitial: true },
      { name: 'ნატალია ჯინჯოლია', specialty: 'კარდიოლოგია', phone: '599 158 738', isInitial: true },
      { name: 'ნანა ჩადუნელი', specialty: 'კარდიოლოგია', phone: '593 145 444', isInitial: true },
      { name: 'სოფიო ნაჭყებია', specialty: 'კარდიოლოგია', phone: '574 730 555', isInitial: true },
      { name: 'ზურაბ ოკუჯავა', specialty: 'კარდიოლოგია', phone: '599 584 646', isInitial: true },
      { name: 'ნათია ჩიქოვანი', specialty: 'კარდიოლოგია', phone: '598 280 843', isInitial: true },
      { name: 'ნინო ჩხაიძე', specialty: 'კარდიოლოგია', phone: '599 073 195', isInitial: true },
      { name: 'ბაქარ ცნობილაძე', specialty: 'კარდიოლოგია', phone: '568 817 537', isInitial: true },
      { name: 'ნინო გიორგაძე', specialty: 'კარდიოლოგია', phone: '577 970 910', isInitial: true },
      { name: 'თინათინ ნაფეტვარიძე', specialty: 'კარდიოლოგია', phone: '598 358 522', isInitial: true }
    ];

    // ============ FILTER + RENDER ============
    function populateSpecialtyFilter() {
      const uniq = [...new Set(allDoctors.map(d => d.specialty).filter(Boolean))]
        .sort((a,b) => a.localeCompare(b,'ka'));

      const sel = document.getElementById('specialty-filter');
      const currentValue = sel.value;
      sel.innerHTML = '<option value="">ყველა სპეციალობა</option>';

      uniq.forEach(s => {
        const o = document.createElement('option');
        o.value = s;
        o.textContent = s;
        sel.appendChild(o);
      });

      // Try to keep selected value if still exists
      const stillExists = [...sel.options].some(o => o.value === currentValue);
      sel.value = stillExists ? currentValue : '';
    }

    function renderDoctors() {
      const searchRaw = document.getElementById('search-input').value || '';
      const search = searchRaw.toLowerCase().trim();
      const searchDigits = normalizePhone(searchRaw);
      const spec = document.getElementById('specialty-filter').value;

      let filtered = allDoctors.filter(d => {
        const name = String(d.name || '').toLowerCase();
        const phoneDigits = normalizePhone(d.phone);

        const matchesSearch =
          !search ||
          name.includes(search) ||
          (searchDigits && phoneDigits.includes(searchDigits));

        const matchesSpecialty = !spec || d.specialty === spec;
        return matchesSearch && matchesSpecialty;
      });

      filtered.sort(
        currentSort === 'name'
          ? (a,b) => String(a.name || '').localeCompare(String(b.name || ''),'ka')
          : (a,b) => String(a.specialty || '').localeCompare(String(b.specialty || ''),'ka')
                    || String(a.name || '').localeCompare(String(b.name || ''),'ka')
      );

      const list = document.getElementById('doctors-list');
      const nores = document.getElementById('no-results');

      if (filtered.length === 0) {
        list.style.display = 'none';
        nores.style.display = 'block';
        return;
      }

      nores.style.display = 'none';
      list.style.display = 'grid';
      list.innerHTML = '';

      filtered.forEach(d => {
        const card = document.createElement('div');
        card.className = 'doctor-card';

        const name = document.createElement('div');
        name.className = 'doctor-name';
        name.textContent = d.name || '';

        const specEl = document.createElement('div');
        specEl.className = 'doctor-specialty';
        specEl.textContent = d.specialty || '';

        const phone = document.createElement('div');
        phone.className = 'doctor-phone';

        const link = document.createElement('a');
        const digits = normalizePhone(d.phone);
        link.href = `tel:+995${digits}`;
        link.textContent = d.phone || '';
        phone.appendChild(link);

        card.append(name, specEl, phone);
        list.appendChild(card);
      });
    }

    // ============ FIREBASE LOADING ============
    function stopListener() {
      if (typeof unsubscribe === 'function') {
        try { unsubscribe(); } catch (_) {}
      }
      unsubscribe = null;
    }

    async function loadDoctorsFromFirebase() {
      setLoading(true);

      // Always start with initial list so UI NEVER becomes empty
      allDoctors = [...initialDoctors];
      populateSpecialtyFilter();
      renderDoctors();

      // If firebase not connected, stop here
      if (!isFirebaseConnected || !db) {
        updateFirebaseStatus('disconnected', 'Firebase არ არის დაკავშირებული');
        setLoading(false);
        return;
      }

      try {
        stopListener();
        const doctorsCollection = collection(db, 'doctors');

        // ✅ Added error handler for onSnapshot
        unsubscribe = onSnapshot(
          doctorsCollection,
          (snapshot) => {
            updateFirebaseStatus('connected');
            isFirebaseConnected = true;

            allDoctors = [...initialDoctors];
            snapshot.forEach((docSnap) => {
              allDoctors.push({
                id: docSnap.id,
                ...docSnap.data(),
                isInitial: false
              });
            });

            console.log(`📋 სულ ჩაიტვირთა ${allDoctors.length} ექიმი (საწყისი: ${initialDoctors.length}, დამატებული: ${snapshot.size})`);
            populateSpecialtyFilter();
            renderDoctors();
            setLoading(false);
          },
          (err) => {
            console.error('❌ onSnapshot error:', err);
            isFirebaseConnected = false;

            // Typical: permission-denied, unavailable, etc.
            const msg = err?.code ? `Firebase შეცდომა: ${err.code}` : 'Firebase შეცდომა';
            updateFirebaseStatus('disconnected', msg);
            showMessage(msg, 'error');

            // Keep initial list visible
            allDoctors = [...initialDoctors];
            populateSpecialtyFilter();
            renderDoctors();
            setLoading(false);
          }
        );

      } catch (error) {
        console.error('❌ ექიმების ჩატვირთვა ვერ მოხერხდა:', error);
        isFirebaseConnected = false;
        updateFirebaseStatus('disconnected', 'მონაცემები ვერ ჩაიტვირთა');
        showMessage('მონაცემების ჩატვირთვა ვერ მოხერხდა', 'error');

        allDoctors = [...initialDoctors];
        populateSpecialtyFilter();
        renderDoctors();
        setLoading(false);
      }
    }

    async function addDoctorToFirebase(doctorData) {
      if (!db) {
        showMessage('Firebase არ არის ინიციალიზებული', 'error');
        return false;
      }

      try {
        const doctorsCollection = collection(db, 'doctors');
        await addDoc(doctorsCollection, {
          ...doctorData,
          createdAt: serverTimestamp()
        });

        console.log('✅ ექიმი წარმატებით დაემატა Firebase-ში');
        showMessage('ექიმი წარმატებით დაემატა!', 'success');
        return true;

      } catch (error) {
        console.error('❌ ექიმის დამატება ვერ მოხერხდა:', error);

        // Show real reason to you
        const msg = error?.code ? `ექიმის დამატება ვერ მოხერხდა: ${error.code}` : 'ექიმის დამატება ვერ მოხერხდა';
        showMessage(msg, 'error');
        updateFirebaseStatus('disconnected', msg);
        return false;
      }
    }

    // ============ MODAL ============
    const modal = document.getElementById('add-doctor-modal');
    const addBtn = document.getElementById('add-doctor-btn');
    const closeBtn = document.getElementsByClassName('close')[0];
    const cancelBtn = document.getElementById('cancel-btn');
    const form = document.getElementById('add-doctor-form');

    addBtn.onclick = function(e) {
      e.preventDefault();
      modal.style.display = 'block';
      form.reset();
      hideErrors();
    };

    closeBtn.onclick = function(e) {
      e.preventDefault();
      modal.style.display = 'none';
    };

    cancelBtn.onclick = function(e) {
      e.preventDefault();
      modal.style.display = 'none';
    };

    window.onclick = function(event) {
      if (event.target === modal) {
        modal.style.display = 'none';
      }
    };

    form.onsubmit = async function(e) {
      e.preventDefault();

      const name = document.getElementById('doctor-name').value.trim();
      const specialty = document.getElementById('doctor-specialty').value;
      const phone = document.getElementById('doctor-phone').value.trim();

      let isValid = true;
      hideErrors();

      if (!name) { showError('name-error'); isValid = false; }
      if (!specialty) { showError('specialty-error'); isValid = false; }
      if (!phone) { showError('phone-error'); isValid = false; }

      if (!isValid) return;

      const newDoctor = { name, specialty, phone };
      const success = await addDoctorToFirebase(newDoctor);

      if (success) {
        modal.style.display = 'none';
        form.reset();
      }
    };

    function showError(id) {
      document.getElementById(id).style.display = 'block';
    }

    function hideErrors() {
      document.querySelectorAll('.error-message').forEach(el => {
        el.style.display = 'none';
      });
    }

    // ============ EVENTS ============
    function toggleSortActive(id) {
      document.querySelectorAll('.sort-btn').forEach(b => b.classList.remove('active'));
      document.getElementById(id).classList.add('active');
    }

    function setupEventListeners() {
      document.getElementById('search-input').addEventListener('input', renderDoctors);
      document.getElementById('specialty-filter').addEventListener('change', renderDoctors);

      document.getElementById('refresh-btn').addEventListener('click', () => {
        loadDoctorsFromFirebase();
      });

      document.getElementById('sort-name').addEventListener('click', () => {
        currentSort = 'name';
        toggleSortActive('sort-name');
        renderDoctors();
      });

      document.getElementById('sort-specialty').addEventListener('click', () => {
        currentSort = 'specialty';
        toggleSortActive('sort-specialty');
        renderDoctors();
      });
    }

    // ============ INIT ============
    function initDirectory() {
      console.log('Initializing directory...');

      setupEventListeners();

      // ✅ Immediately show initial list + fill specialties
      allDoctors = [...initialDoctors];
      populateSpecialtyFilter();
      renderDoctors();

      // Then try firebase sync
      loadDoctorsFromFirebase();
    }

    if (document.readyState === 'loading') {
      document.addEventListener('DOMContentLoaded', initDirectory);
    } else {
      initDirectory();
    }
  </script>
</body>
</html>
