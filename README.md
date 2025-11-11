<html lang="ka">
 <head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Medical Directory</title>
  <script src="/_sdk/element_sdk.js"></script>
  <style>
    /* ფერთა პალიტრა */
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
        --white: #ffffff;
    }
    body {
      box-sizing: border-box;
      margin: 0;
      padding: 0;
      font-family: 'Inter', -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;
      background: var(--bg-light);
      color: var(--text-color);
      min-height: 100vh;
      line-height: 1.6;
      display: flex;
      flex-direction: column;
    }
    *, *::before, *::after { box-sizing: inherit; }
    .container { max-width: 1200px; margin: 0 auto; padding: 32px 24px; flex: 1; }
    header { text-align: center; margin-bottom: 40px; border-bottom: 2px solid var(--border-light); padding-bottom: 20px; }
    h1 { font-size: 36px; font-weight: 800; margin: 0 0 4px; letter-spacing: -0.8px; color: var(--dark-blue); }
    .subtitle { font-size: 18px; color: var(--light-blue); margin: 0; font-weight: 500; }

    /* Controls */
    .controls { display: flex; flex-wrap: wrap; gap: 20px; margin-bottom: 30px; align-items: center; }
    .search-box, .filter-box { flex: 1; min-width: 250px; }
    .search-box input, .filter-box select, #auth-password-input {
      width: 100%; padding: 12px 16px; border: 1px solid #ced4da; border-radius: 8px; font-size: 16px;
      font-family: inherit; background: var(--white); color: var(--text-color);
      transition: border-color 0.2s, box-shadow 0.2s;
    }
    .search-box input:focus, .filter-box select:focus, #auth-password-input:focus {
      outline: none; border-color: var(--primary-blue); box-shadow: 0 0 0 3px rgba(0, 119, 182, 0.15);
    }
    .filter-box select { cursor: pointer; }
    .action-buttons { display: flex; gap: 10px; }

    /* Buttons */
    button {
      padding: 12px 20px; border: 2px solid var(--primary-blue); background: var(--primary-blue);
      color: var(--white); font-size: 16px; font-weight: 600; border-radius: 8px; cursor: pointer;
      font-family: inherit; transition: all 0.2s; box-shadow: 0 2px 4px rgba(0,0,0,0.1);
    }
    button:hover { background: var(--dark-blue); border-color: var(--dark-blue); transform: translateY(-2px); box-shadow: 0 6px 15px rgba(2,62,138,0.25); }
    #print-btn { background: var(--white); color: var(--primary-blue); }
    #print-btn:hover { background: var(--border-light); color: var(--dark-blue); border-color: var(--dark-blue); }
    button:disabled { opacity: 0.6; cursor: not-allowed; transform: none; box-shadow: none; }

    /* Sort */
    .sort-controls { display: flex; gap: 10px; margin-bottom: 24px; }
    .sort-btn {
      padding: 8px 16px; font-size: 14px; border: 1px solid var(--border-light);
      background: var(--white); color: var(--primary-blue); transition: all 0.2s;
    }
    .sort-btn:hover { background: #f0f4f8; }
    .sort-btn.active { background: var(--primary-blue); color: var(--white); border-color: var(--primary-blue); }
    .sort-btn.active:hover { background: var(--dark-blue); }

    /* Loading & No Results */
    .loading { text-align: center; padding: 40px; font-size: 18px; color: var(--primary-blue); font-weight: 500; }
    .spinner {
      display: inline-block; width: 24px; height: 24px; border: 3px solid rgba(0,119,182,0.2);
      border-top: 3px solid var(--primary-blue); border-radius: 50%; animation: spin 1s linear infinite;
      margin-right: 10px; vertical-align: middle;
    }
    @keyframes spin { 0% { transform: rotate(0deg); } 100% { transform: rotate(360deg); } }
    .doctors-list { display: grid; grid-template-columns: repeat(auto-fill, minmax(300px, 1fr)); gap: 20px; }
    .no-results { text-align: center; padding: 60px; font-size: 18px; color: var(--light-blue); font-weight: 500; }

    /* Doctor Card */
    .doctor-card {
      border: 1px solid var(--border-light); padding: 24px; border-radius: 12px; background: var(--white);
      box-shadow: 0 4px 12px rgba(0,0,0,0.05); transition: all 0.3s cubic-bezier(0.25,0.8,0.25,1);
    }
    .doctor-card:hover {
      box-shadow: 0 8px 25px rgba(0,119,182,0.15); transform: translateY(-3px); border-color: var(--primary-blue);
    }
    .doctor-name { font-size: 20px; font-weight: 700; margin: 0 0 6px; color: var(--dark-blue); letter-spacing: -0.3px; }
    .doctor-specialty { font-size: 15px; color: var(--light-blue); margin: 0 0 16px; font-weight: 600; }
    .doctor-phone { font-size: 16px; font-weight: 500; }
    .doctor-phone a {
      color: var(--primary-blue); text-decoration: none; border-bottom: 1px dashed var(--primary-blue);
      transition: all 0.2s;
    }
    .doctor-phone a:hover { color: var(--dark-blue); border-bottom: 2px solid var(--dark-blue); }

    /* AUTH SCREEN */
    .auth-overlay {
        position: fixed; top: 0; left: 0; width: 100%; height: 100%; background: var(--white);
        display: flex; justify-content: center; align-items: center; z-index: 1000;
        transition: opacity 0.4s ease; opacity: 1;
    }
    .auth-box {
        background: var(--white); padding: 40px; border-radius: 12px;
        box-shadow: 0 10px 30px rgba(0,0,0,0.1); max-width: 400px; width: 90%;
        text-align: center; border: 1px solid var(--border-light);
    }
    .auth-box h2 { color: var(--dark-blue); margin: 0 0 20px; font-size: 24px; font-weight: 700; }
    .auth-box button { width: 100%; margin-top: 20px; }
    #auth-error-message { color: var(--error-red); margin-top: 10px; font-size: 14px; font-weight: 600; min-height: 20px; }
    .auth-logo { width: 100%; max-width: 300px; margin: 0 auto 30px; }
    .auth-logo img { width: 100%; height: auto; display: block; }

    /* FOOTER */
    .footer {
      margin-top: auto;
      text-align: center;
      padding: 20px 0;
      font-size: 14px;
      color: #6c757d;
      background: var(--bg-light);
      border-top: 1px solid var(--border-light);
      font-weight: 500;
    }
    .footer p {
      margin: 0;
    }

    /* Print */
    @media print {
      .auth-overlay, .controls, .action-buttons, .sort-controls, button, .footer { display: none !important; }
      .container { max-width: 100%; padding: 0; }
      .doctor-card { break-inside: avoid; box-shadow: none !important; border: 1px solid #ccc !important; margin-bottom: 15px; }
      body { background: #ffffff; }
    }

    /* Responsive */
    @media (max-width: 768px) {
      .container { padding: 20px 16px; }
      h1 { font-size: 28px; }
      .subtitle { font-size: 16px; }
      .controls { flex-direction: column; gap: 15px; }
      .search-box, .filter-box { width: 100%; min-width: 100%; }
      .action-buttons { width: 100%; justify-content: space-between; }
      .action-buttons button { flex: 1; }
      .sort-controls { flex-wrap: wrap; gap: 8px; justify-content: center; }
      .doctors-list { grid-template-columns: 1fr; }
      .auth-box { padding: 30px 20px; width: 95%; }
      .auth-logo { max-width: 250px; margin-bottom: 20px; }
    }
  </style>
  <style>@view-transition { navigation: auto; }</style>
  <script src="/_sdk/data_sdk.js" type="text/javascript"></script>
  <script src="https://cdn.tailwindcss.com" type="text/javascript"></script>
 </head>
 <body>

  <!-- AUTH SCREEN -->
  <div id="auth-overlay" class="auth-overlay">
    <div class="auth-box">
      <div class="auth-logo">
        <img src="./tm_center_logo.png" alt="TM Center კლინიკის ლოგო">
      </div>
      <h2>ავტორიზაცია</h2>
      <p style="color: var(--dark-blue); font-size: 15px; margin-bottom: 25px;">შეიყვანეთ პაროლი</p>
      <input type="password" id="auth-password-input" placeholder="პაროლი" aria-label="Enter password">
      <div id="auth-error-message"></div>
      <button id="auth-login-btn">შესვლა</button>
    </div>
  </div>

  <!-- MAIN CONTENT -->
  <div id="main-content" style="display: none;">
    <div class="container">
      <header>
        <h1 id="clinic-title">თბილისის სახელმწიფო სამედიცინო უნივერსიტეტისა და ინგოროყვას მაღალი სამედიცინო ტექნოლოგიების საუნივერსიტეტო კლინიკა</h1>
        <p class="subtitle" id="clinic-subtitle">ექიმების სატელეფონო სია</p>
      </header>
      <div class="controls">
        <div class="search-box">
          <input type="text" id="search-input" placeholder="ძებნა სახელით ან გვარით..." aria-label="Search doctors">
        </div>
        <div class="filter-box">
          <select id="specialty-filter" aria-label="Filter by specialty">
            <option value="">ყველა სპეციალობა</option>
          </select>
        </div>
        <div class="action-buttons">
          <button id="refresh-btn"><span id="refresh-text">განახლება</span></button>
          <button id="print-btn" onclick="window.print()"><span id="print-text">ბეჭდვა</span></button>
        </div>
      </div>
      <div class="sort-controls">
        <button class="sort-btn active" id="sort-name">A to Z (სახელი)</button>
        <button class="sort-btn" id="sort-specialty">სპეციალობა</button>
      </div>
      <div id="loading" class="loading" style="display: none;"><span class="spinner"></span> მონაცემები იტვირთება...</div>
      <div id="doctors-list" class="doctors-list"></div>
      <div id="no-results" class="no-results" style="display: none;">შედეგები არ მოიძებნა</div>
    </div>
  </div>

  <!-- FOOTER -->
  <footer class="footer">
    <p>made by IMED🩺</p>
  </footer>

  <script>
    // === 1. პაროლი ===
    const CORRECT_PASSWORD = "med123"; // შეცვალე შენი პაროლით

    // === 2. კონფიგურაცია ===
    const defaultConfig = {
      clinic_title: 'თბილისის სახელმწიფო სამედიცინო უნივერსტიტეტისა და ინგოროყვას მაღალი სამედიცინო ტექნოლოგიების საუნივერსიტეტო კლინიკა',
      clinic_subtitle: 'ექიმების სატელეფონო სია',
      refresh_button_text: 'განახლება',
      print_button_text: 'ბეჭდვა',
      search_placeholder: 'ძებნა სახელით ან გვარით...',
      primary_color: '#0077b6',
      background_color: '#f8f9fa',
      text_color: '#023e8a',
      secondary_text_color: '#00b4d8',
      border_color: '#0077b6',
      font_family: 'Inter',
      font_size: 16
    };
    let config = { ...defaultConfig };
    let allDoctors = [];
    let currentSort = 'name';

    // === 3. ექიმების მონაცემები ===
    const sampleDoctors = [
      { name: 'პაატა ბარათაშვილი', specialty: 'რადიოლოგი', phone: '593 311 748' },
      { name: 'ვაჟა თავბერიძე', specialty: 'რადიოლოგი', phone: '551 470 471' },
      { name: 'მაია', specialty: 'რენტგენი', phone: '557 654 351' },
      { name: 'ნინო', specialty: 'რენტგენი', phone: '599 400 311' },
      { name: 'ნაზი', specialty: 'რენტგენი', phone: '555 181 801' },
      { name: 'მარიამი', specialty: 'რენტგენი', phone: '598 100 644' },
     { name: 'ნიკა მაჩაიძე', specialty: 'CT ოპერატორი', phone: '‭598 295 798‬' },
     { name: 'მარიამი', specialty: 'CT ოპერატორი', phone: '‭599 216 624‬' },
     { name: 'ზურა ქოჩერაშვილი', specialty: 'CT ოპერატორი', phone: '‭557 767 362‬' },
     { name: 'მაია დემურიშვილი', specialty: 'CT რადიოლოგი', phone: '‭555 258 800‬' },
      { name: 'ვალერიანე უხურგუნაშვილი', specialty: 'CT რადიოლოგი', phone: '‭‭558 333 455‬‬' },
     { name: 'ცისია კახაძე', specialty: 'CT რადიოლოგი', phone: '‭‭599 407 560‬' },
      { name: 'ჯუბა ნაზარაშვილი', specialty: 'CT ოპერატორი', phone: '‭571 036 317‬' },
      { name: 'მანანა გოგოლაძე', specialty: 'ექოსკოპია', phone: '577 450 049' },
      { name: 'ანა ინგოროყვა', specialty: 'ექოსკოპია', phone: '599 222 201' },
      { name: 'მარიამ გავაშელი', specialty: 'ექოსკოპია', phone: '544 447 346' },
      { name: 'თამარ გოგელია', specialty: 'ექოსკოპია', phone: '557 424 363' },
      { name: 'ირინა მოდებაძე', specialty: 'ექოსკოპია', phone: '577 090 967' },
      { name: 'ლაბორატორია', specialty: 'ლაბორატორია', phone: '577 101 949' },
      { name: 'ირაკლი დევიძე', specialty: 'ყბა-სახის ქირურგია', phone: '597 03 05 40' },
      { name: 'გიორგი გვენეტაძე', specialty: 'ყბა-სახის ქირურგია', phone: '599 62 99 91' },
      { name: 'ერეკლე გელაშვილი', specialty: 'ყბა-სახის ქირურგია', phone: '597 02 20 99' },
      { name: 'ნუნუკა გურაბანიძე', specialty: 'ყბა-სახის ქირურგია', phone: '551 159 797' },
      { name: 'გრიგოლ ჯავახაძე', specialty: 'ყბა-სახის ქირურგია', phone: '597 098 116' },
      { name: 'შალვა ჭოველიძე', specialty: 'უროლოგია', phone: '577 460 025' },
      { name: 'ნიკოლოზ გვარამია', specialty: 'უროლოგია', phone: '597 774 091' },
      { name: 'ვუგარ სადიკოვი', specialty: 'უროლოგია', phone: '557 175 005' },
      { name: 'ნანა გოგოხია', specialty: 'უროლოგია', phone: '557 497 474' },
      { name: 'მარიკა ყურაშვილი', specialty: 'უროლოგია', phone: '555 213 650' },
      { name: 'ზაური თაქთაქიშილი', specialty: 'უროლოგია', phone: '551 591 774' },
      { name: 'გიგი ორაგველიძე', specialty: 'უროლოგია', phone: '511 282 879' },
     { name: 'გიორგი ხიზანიშვილი', specialty: 'ტრავმატოლოგია', phone: '‭595 914 096‬' },
      { name: 'კახა გოშაძე', specialty: 'ტრავმატოლოგია', phone: '598 787 859‬‬' },
     { name: 'ზურა ჩხარტიშვილი', specialty: 'ტრავმატოლოგია', phone: '599 055 181‬' },
     { name: 'ნიკა ლომიძე', specialty: 'ტრავმატოლოგია', phone: '‭599 808 191‬' },
     { name: 'ნიკა რაზმაძე', specialty: 'ტრავმატოლოგია', phone: '‭‭579 775 674‬' },
      { name: 'გურამ ჩაჩუა', specialty: 'ნეირო ქირურგია', phone: '579 031 178' },
      { name: 'მიხეილ გურასპიშვილი', specialty: 'ნეირო ქირურგია', phone: '555 191 378' },
      { name: 'ოთარ გახოკია', specialty: 'ნეირო ქირურგია', phone: '558 344 233' },
      { name: 'არჩილ წიკლაური', specialty: 'ნეირო ქირურგია', phone: '558 566 848' },
      { name: 'ლუკა ლეკაშვილი', specialty: 'ნეირო ქირურგია', phone: '595 455 135' },
      { name: 'ლუკა გოგოტიშვილი', specialty: 'ნეირო ქირურგია', phone: '592 861 741' },
      { name: 'კორპორატიული', specialty: 'ნეირო ქირურგია', phone: '511 453 571' },
      { name: 'ნეირორეანიმაცია', specialty: 'ნეირო ქირურგია', phone: '511 453 576' },
      { name: 'ნინო ხარაიშვილი', specialty: 'ნევროლოგია', phone: '593 151 588' },
      { name: 'ნათია ხაჩიძე', specialty: 'ნევროლოგია', phone: '598 61 06 24' },
      { name: 'ალექსი მაღლაკელიძე', specialty: 'ნევროლოგია', phone: '591 06 52 37' },
      { name: 'თამთა კარანაძე', specialty: 'ნევროლოგია', phone: '577 395 080' },
      { name: 'ჟანა', specialty: 'ნევროლოგია', phone: '579 379 252' },
      { name: 'ქრისტინე დვალაძე', specialty: 'ნევროლოგია', phone: '568 03 03 36' },
      { name: 'ნათია კურტანიძე', specialty: 'ნევროლოგია', phone: '599 70 57 33' },
      { name: 'ანა შუბითიძე', specialty: 'ნევროლოგია', phone: '555 37 59 68' },
      { name: 'ანა ქურხული', specialty: 'ნევროლოგია', phone: '568 908 466' },
      { name: 'ირინა ჯაჯანიძე', specialty: 'გინეკოლოგია', phone: '599 90 14 58' },
      { name: 'რუსუდან ფიცხელაური', specialty: 'გინეკოლოგია', phone: '599 67 61 40' },
      { name: 'ნინო ხათრიძე', specialty: 'გინეკოლოგია', phone: '598 48 21 42' },
      { name: 'დიანა მირზაშვილი', specialty: 'გინეკოლოგია', phone: '599 90 42 98' },
      { name: 'თინა ჩალიგავა', specialty: 'გინეკოლოგია', phone: '599 13 07 08' },
      { name: 'ნინო შარაშენიძე', specialty: 'ჰემატოლოგია', phone: '599 91 49 91' },
      { name: 'ია მალაშხია', specialty: 'ჰემატოლოგია', phone: '599 490 305' },
      { name: 'შამო მუსაევი', specialty: 'ჰემატოლოგია', phone: '557 949 226' },
      { name: 'თაკო აზიკური', specialty: 'ჰემატოლოგია', phone: '593 545 233' },
      { name: 'ნატალია ნადიკაშვილი', specialty: 'ჰემატოლოგია', phone: '577 222 970' },
      { name: 'ჯონდი ჭავჭანიძე', specialty: 'ენდოსკოპია', phone: '577 453 405' },
      { name: 'დავით გობეჯიშვილი', specialty: 'ენდოსკოპია', phone: '599 933 584' },
      { name: 'თეიმურაზ სამადაშვილი', specialty: 'ენდოსკოპია', phone: '598 22 22 46' },
      { name: 'ირაკლი შეკლაშვილი', specialty: 'ენდოსკოპია', phone: '577 339 956' },
      { name: 'მარიკა წერეთელი', specialty: 'ინფექციური სნეულებები', phone: '593 362 987' },
      { name: 'ნუცა დონაძე', specialty: 'ინფექციური სნეულებები', phone: '599 89 08 29' },
      { name: 'თაკო ზაზაძე', specialty: 'ინფექციური სნეულებები', phone: '597 777 113' },
      { name: 'თამარ წერეთელი', specialty: 'ინფექციური სნეულებები', phone: '555 558 333' },
      { name: 'ია ბაღაშვილი', specialty: 'ინფექციური სნეულებები', phone: '577 58 82 05' },
      { name: 'ირმა მარკოიძე', specialty: 'ინფექციური სნეულებები', phone: '599 470 228' },
      { name: 'ციცი მაღლაფერიძე', specialty: 'ინფექციური სნეულებები', phone: '579 70 60 81' },
      { name: 'ნინო წურწუმია', specialty: 'ინფექციური სნეულებები', phone: '557 58 78 34' },
     { name: 'ნინო აბულაძე', specialty: 'ინფექციური სნეულებები', phone: '599 060 194' },
      { name: 'თამარი ტურიაშვილი', specialty: 'ინფექციური სნეულებები', phone: '598 005 186' },
      { name: 'ირინა კილაძე', specialty: 'ინფექციური სნეულებები', phone: '599 88 35 77' },
      { name: 'ეკატერინე მარკოზია', specialty: 'ინფექციური სნეულებები', phone: '555 739 633' },
      { name: 'ლაშა სარალიღე', specialty: 'ზოგადი ქირურგია', phone: '599 977 762' },
      { name: 'მაია ლობჟანიძე', specialty: 'ზოგადი ქირურგია', phone: '577 671 710' },
      { name: 'დავით ვარდოსანიძე', specialty: 'ზოგადი ქირურგია', phone: '577 671 705' },
      { name: 'ზაზა მანელიძე', specialty: 'ზოგადი ქირურგია', phone: '595 582 876' },
      { name: 'ირაკლი კაჭახიძე', specialty: 'ზოგადი ქირურგია', phone: '577 671 707' },
      { name: 'ლალი ახმეტელი', specialty: 'ზოგადი ქირურგია', phone: '577 553 311' },
      { name: 'ლია საგინაშვილი', specialty: 'ზოგადი ქირურგია', phone: '599 503 567' },
      { name: 'ბესო ირემაშვილი', specialty: 'ზოგადი ქირურგია', phone: '595 300 719' },
      { name: 'ონისე ტყეშელაშვილი', specialty: 'ზოგადი ქირურგია', phone: '574 219 219' },
      { name: 'გიორგი შუბითიძე', specialty: 'ზოგადი ქირურგია', phone: '595 418 040' },
      { name: 'გუგა ზაალიშვილი', specialty: 'თორაკო ქირურგია', phone: '577 459 556' },
      { name: 'რობერტი გობეჩია', specialty: 'თორაკო ქირურგია', phone: '599 931 120' },
      { name: 'ვასო ბაბიაშვილი', specialty: 'თორაკო ქირურგია', phone: '557 752 565' },
      { name: 'დათო მარკოზია', specialty: 'თორაკო ქირურგია', phone: '593 100 176' },
      { name: 'ლევან ქაცარავა', specialty: 'თორაკო ქირურგია', phone: '593 696 743' },
      { name: 'ირინა სვიანაძე', specialty: 'პულმონოლოგია', phone: '555 539 733' },
      { name: 'ლანა ბერია', specialty: 'პულმონოლოგია', phone: '598 358 377' },
      { name: 'ნინო ღრუბელაშვილი', specialty: 'ზოგადი რეანიმაცია', phone: '599 943 008' },
      { name: 'ასიკო ენუქიძე', specialty: 'ზოგადი რეანიმაცია', phone: '577 101 910' },
      { name: 'თიკო კუჭავა', specialty: 'ზოგადი რეანიმაცია', phone: '599 425 646' },
      { name: 'დათო კახიძე', specialty: 'ზოგადი რეანიმაცია', phone: '598 535 337' },
      { name: 'შორენა მურმანიშვილი', specialty: 'ზოგადი რეანიმაცია', phone: '599 361 288' },
      { name: 'თამუნა ხუციშვილი', specialty: 'ზოგადი რეანიმაცია', phone: '599 141 380' },
      { name: 'ლიკა ქობლიანიძე', specialty: 'ზოგადი რეანიმაცია', phone: '599 313 345' },
      { name: 'ნათია ჯიყაშვილი', specialty: 'ზოგადი რეანიმაცია', phone: '592 058 180' },
      { name: 'ვახტანგ ჩიქოვანი', specialty: 'ზოგადი რეანიმაცია', phone: '599 420 576' },
      { name: 'მარიამ მერებაშვილი', specialty: 'ზოგადი რეანიმაცია', phone: '598 477 662' },
      { name: 'დათო მამინაშვილი', specialty: 'შინაგანი მედიცინა', phone: '579 49 15 51' },
      { name: 'ნიკო გოგალაძე', specialty: 'შინაგანი მედიცინა', phone: '568 96 13 85' },
      { name: 'გვანცა ხაჩიაშვილი', specialty: 'შინაგანი მედიცინა', phone: '571 187 920' },
      { name: 'ნათია ეფრემიძე', specialty: 'შინაგანი მედიცინა', phone: '557 752 842' },
      { name: 'ნინო მიტიჩაშვილი', specialty: 'ჰეპატოლოგია', phone: '579 559 558' },
      { name: 'მარიამ ბერიძე', specialty: 'ნეფროლოგია', phone: '599 758 607' },
      { name: 'მაკა ტაბაღუა', specialty: 'ნეფროლოგია', phone: '598 77 88 12' },
      { name: 'მარიამ გიუაშვილი', specialty: 'ნეფროლოგია', phone: '551 45 21 21' },
      { name: 'რუსუდან რუსია', specialty: 'ნეფროლოგია', phone: '599 11 15 11' },
      { name: 'ნორა სარიშვილი', specialty: 'ნეფროლოგია', phone: '593 128 485' },
      { name: 'სალომე დარასელია', specialty: 'ნეფროლოგია', phone: '574 54 37 37' },
      { name: 'გიორგი გაზდელიანი', specialty: 'ნეფროლოგია', phone: '591 000 604' },
      { name: 'ანა ჭიქაბერიძე', specialty: 'ნეფროლოგია', phone: '599 103 106' },
     { name: 'ხაიალ დემურჩიევ', specialty: 'ნეფროლოგია', phone: '577 591 644' },
      { name: 'ნინო ბუაძე', specialty: 'ნეფროლოგია', phone: '593 494 995' },
      { name: 'თეონა ხელაშვილი', specialty: 'ნეფროლოგია', phone: '557 438 626' },
      { name: 'გვანცა მეცხვარიშვილი', specialty: 'ნეფროლოგია', phone: '597 777 991' },
      { name: 'თამარ კასრაძე', specialty: 'ნეფროლოგია', phone: '593 329 900' },
      { name: 'ნონა ბაბუციძე', specialty: 'ნეფროლოგია', phone: '555 595 550' },
      { name: 'თამარ თევდორაძე', specialty: 'ნეფროლოგია', phone: '551 770 505' },
      { name: 'ქეთევან დალაქიშვილი', specialty: 'ნეფროლოგია', phone: '599 194 353' },
      { name: 'თამარ ბაგაშვილი', specialty: 'ნეფროლოგია', phone: '593 934 241' },
      { name: 'ქეთი კაპანაძე', specialty: 'ნეფროლოგია', phone: '598 232 177' },
      { name: 'ზურაბ გოგინაშვილი', specialty: 'ანგიოქირურგია', phone: '599 11 34 57' },
      { name: 'ზურაბ გოგიჩაშვილი', specialty: 'ანგიოქირურგია', phone: '599 55 80 60' },
      { name: 'დათო ბაბილოძე', specialty: 'ანგიოქირურგია', phone: '599 520 938' },
      { name: 'ნატალია ჯინჯოლია', specialty: 'კარდიოლოგია', phone: '599 158 738' },
      { name: 'ნანა ჩადუნელი', specialty: 'კარდიოლოგია', phone: '593 145 444' },
      { name: 'სოფიო ნაჭყებია', specialty: 'კარდიოლოგია', phone: '574 730 555' },
      { name: 'ზურაბ ოკუჯავა', specialty: 'კარდიოლოგია', phone: '599 584 646' },
      { name: 'ნათია ჩიქოვანი', specialty: 'კარდიოლოგია', phone: '598 280 843' },
      { name: 'ნინო ჩხაიძე', specialty: 'კარდიოლოგია', phone: '599 073 195' },
      { name: 'ბაქარ ცნობილაძე', specialty: 'კარდიოლოგია', phone: '568 817 537' },
      { name: 'ნინო გიორგაძე', specialty: 'კარდიოლოგია', phone: '577 970 910' },
      { name: 'თინათინ ნაფეტვარიძე', specialty: 'კარდიოლოგია', phone: '598 358 522' }
    ];

    // === 4. ავტორიზაცია ===
    function handleLogin(e) {
        if (e) e.preventDefault();
        const input = document.getElementById('auth-password-input');
        const error = document.getElementById('auth-error-message');
        const overlay = document.getElementById('auth-overlay');
        const main = document.getElementById('main-content');
        const pass = input.value.trim();
        if (pass === CORRECT_PASSWORD) {
            error.textContent = '';
            overlay.style.opacity = '0';
            setTimeout(() => {
                overlay.style.display = 'none';
                main.style.display = 'block';
                initDirectory();
            }, 400);
        } else {
            error.textContent = 'არასწორი პაროლი!';
            input.value = '';
            input.focus();
        }
    }

    // === 5. ინიციალიზაცია ===
    function initDirectory() {
        setupEventListeners();
        fetchDoctors();
        if (window.elementSdk) {
            window.elementSdk.init({ defaultConfig, onConfigChange, mapToCapabilities, mapToEditPanelValues });
        }
    }

    function setupEventListeners() {
        document.getElementById('search-input').addEventListener('input', renderDoctors);
        document.getElementById('specialty-filter').addEventListener('change', renderDoctors);
        document.getElementById('refresh-btn').addEventListener('click', fetchDoctors);
        document.getElementById('sort-name').addEventListener('click', () => {
            currentSort = 'name'; toggleSortActive('sort-name'); renderDoctors();
        });
        document.getElementById('sort-specialty').addEventListener('click', () => {
            currentSort = 'specialty'; toggleSortActive('sort-specialty'); renderDoctors();
        });
    }

    function toggleSortActive(id) {
        document.querySelectorAll('.sort-btn').forEach(b => b.classList.remove('active'));
        document.getElementById(id).classList.add('active');
    }

    // === 6. მონაცემები ===
    async function fetchDoctors() {
        const loading = document.getElementById('loading');
        const list = document.getElementById('doctors-list');
        const btn = document.getElementById('refresh-btn');
        loading.style.display = 'block'; list.style.display = 'none'; btn.disabled = true;
        try {
            await new Promise(r => setTimeout(r, 600));
            allDoctors = [...sampleDoctors];
            populateSpecialtyFilter();
            renderDoctors();
        } catch { allDoctors = [...sampleDoctors]; populateSpecialtyFilter(); renderDoctors(); }
        finally { loading.style.display = 'none'; btn.disabled = false; }
    }

    function populateSpecialtyFilter() {
        const uniq = [...new Set(allDoctors.map(d => d.specialty))].sort((a,b) => a.localeCompare(b,'ka'));
        const sel = document.getElementById('specialty-filter');
        const cur = sel.value;
        sel.innerHTML = '<option value="">ყველა სპეციალობა</option>';
        uniq.forEach(s => { const o = document.createElement('option'); o.value = s; o.textContent = s; sel.appendChild(o); });
        if (cur && uniq.includes(cur)) sel.value = cur;
    }

    function renderDoctors() {
        const search = document.getElementById('search-input').value.toLowerCase();
        const spec = document.getElementById('specialty-filter').value;
        let filtered = allDoctors.filter(d => d.name.toLowerCase().includes(search) && (!spec || d.specialty === spec));
        filtered.sort(currentSort === 'name' ? (a,b) => a.name.localeCompare(b.name,'ka') : (a,b) => a.specialty.localeCompare(b.specialty,'ka') || a.name.localeCompare(b.name,'ka'));
        const list = document.getElementById('doctors-list');
        const nores = document.getElementById('no-results');
        if (filtered.length === 0) { list.style.display = 'none'; nores.style.display = 'block'; return; }
        nores.style.display = 'none'; list.style.display = 'grid'; list.innerHTML = '';
        const fs = config.font_size || 16;
        const ff = config.font_family || 'Inter';
        const stack = '-apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, sans-serif';
        filtered.forEach(d => {
            const card = document.createElement('div'); card.className = 'doctor-card'; card.style.borderColor = config.border_color || '#0077b6';
            const name = document.createElement('div'); name.className = 'doctor-name'; name.textContent = d.name;
            name.style.fontFamily = `${ff}, ${stack}`; name.style.fontSize = `${fs * 1.25}px`; name.style.color = config.text_color || '#023e8a';
            const specEl = document.createElement('div'); specEl.className = 'doctor-specialty'; specEl.textContent = d.specialty;
            specEl.style.fontFamily = `${ff}, ${stack}`; specEl.style.fontSize = `${fs - 1}px`; specEl.style.color = config.secondary_text_color || '#00b4d8';
            const phone = document.createElement('div'); phone.className = 'doctor-phone'; phone.style.fontFamily = `${ff}, ${stack}`; phone.style.fontSize = `${fs}px`;
            const link = document.createElement('a'); link.href = `tel:+995${d.phone.replace(/\s/g,'')}`; link.textContent = d.phone;
            link.style.color = config.border_color || '#0077b6'; link.style.borderBottomColor = config.border_color || '#0077b6';
            phone.appendChild(link); card.append(name, specEl, phone); list.appendChild(card);
        });
    }

    // === 7. SDK კონფიგურაცია ===
    async function onConfigChange(newConfig) {
        config = { ...config, ...newConfig };
        document.documentElement.style.setProperty('--primary-blue', config.primary_color || defaultConfig.primary_color);
        document.documentElement.style.setProperty('--dark-blue', config.text_color || defaultConfig.text_color);
        document.documentElement.style.setProperty('--light-blue', config.secondary_text_color || defaultConfig.secondary_text_color);
        document.documentElement.style.setProperty('--bg-light', config.background_color || defaultConfig.background_color);
        document.documentElement.style.setProperty('--border-color', config.border_color || defaultConfig.border_color);
        document.getElementById('clinic-title').textContent = config.clinic_title || defaultConfig.clinic_title;
        document.getElementById('clinic-subtitle').textContent = config.clinic_subtitle || defaultConfig.clinic_subtitle;
        document.getElementById('refresh-text').textContent = config.refresh_button_text || defaultConfig.refresh_button_text;
        document.getElementById('print-text').textContent = config.print_button_text || defaultConfig.print_button_text;
        document.getElementById('search-input').placeholder = config.search_placeholder || defaultConfig.search_placeholder;
        document.body.style.fontFamily = `${config.font_family || 'Inter'}, ${stack}`;
        document.body.style.fontSize = `${config.font_size || 16}px`;
        renderDoctors();
    }

    function mapToCapabilities(cfg) {
        return {
            recolorables: [
                { get: () => cfg.background_color || defaultConfig.background_color, set: v => { cfg.background_color = v; if (window.elementSdk) window.elementSdk.setConfig({ background_color: v }); } },
                { get: () => cfg.border_color || defaultConfig.border_color, set: v => { cfg.border_color = v; if (window.elementSdk) window.elementSdk.setConfig({ border_color: v }); } },
                { get: () => cfg.text_color || defaultConfig.text_color, set: v => { cfg.text_color = v; if (window.elementSdk) window.elementSdk.setConfig({ text_color: v }); } },
                { get: () => cfg.secondary_text_color || defaultConfig.secondary_text_color, set: v => { cfg.secondary_text_color = v; if (window.elementSdk) window.elementSdk.setConfig({ secondary_text_color: v }); } },
                { get: () => cfg.primary_color || defaultConfig.primary_color, set: v => { cfg.primary_color = v; if (window.elementSdk) window.elementSdk.setConfig({ primary_color: v }); } }
            ],
            borderables: [],
            fontEditable: { get: () => cfg.font_family || defaultConfig.font_family, set: v => { cfg.font_family = v; if (window.elementSdk) window.elementSdk.setConfig({ font_family: v }); } },
            fontSizeable: { get: () => cfg.font_size || defaultConfig.font_size, set: v => { cfg.font_size = v; if (window.elementSdk) window.elementSdk.setConfig({ font_size: v }); } }
        };
    }

    function mapToEditPanelValues(cfg) {
        return new Map([
            ['clinic_title', cfg.clinic_title || defaultConfig.clinic_title],
            ['clinic_subtitle', cfg.clinic_subtitle || defaultConfig.clinic_subtitle],
            ['refresh_button_text', cfg.refresh_button_text || defaultConfig.refresh_button_text],
            ['print_button_text', cfg.print_button_text || defaultConfig.print_button_text],
            ['search_placeholder', cfg.search_placeholder || defaultConfig.search_placeholder]
        ]);
    }

    // === 8. გაშვება ===
    document.addEventListener('DOMContentLoaded', () => {
        const loginBtn = document.getElementById('auth-login-btn');
        const input = document.getElementById('auth-password-input');
        if (loginBtn) loginBtn.addEventListener('click', handleLogin);
        if (input) {
            input.focus();
            input.addEventListener('keypress', e => { if (e.key === 'Enter') handleLogin(e); });
        }
    });
  </script>
 </body>
</html>
