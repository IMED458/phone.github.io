<html lang="ka">
 <head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Medical Directory</title>
  <script src="/_sdk/element_sdk.js"></script>
  <style>
    /* ფერთა პალიტრა: */
    :root {
        --primary-blue: #0077b6;
        --dark-blue: #023e8a;
        --light-blue: #00b4d8;
        --bg-light: #f8f9fa;
        --border-light: #e0eaf4;
        --text-color: var(--dark-blue);
        --secondary-text-color: var(--light-blue);
        --border-color: var(--primary-blue);
        --error-red: #dc3545; /* წითელი შეცდომისთვის */
        --white: #ffffff; /* თეთრი ფერი */
    }

    body {
      box-sizing: border-box;
      margin: 0;
      padding: 0;
      font-family: 'Inter', -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;
      background: var(--bg-light);
      color: var(--text-color);
      min-height: 100%;
      line-height: 1.6;
    }

    *, *::before, *::after {
      box-sizing: inherit;
    }

    .container {
      max-width: 1200px;
      margin: 0 auto;
      padding: 32px 24px;
    }

    header {
      text-align: center;
      margin-bottom: 40px;
      border-bottom: 2px solid var(--border-light);
      padding-bottom: 20px;
    }

    h1 {
      font-size: 36px;
      font-weight: 800;
      margin: 0 0 4px 0;
      letter-spacing: -0.8px;
      color: var(--dark-blue);
    }

    .subtitle {
      font-size: 18px;
      color: var(--light-blue);
      margin: 0;
      font-weight: 500;
    }

    /* Controls Section */
    .controls {
      display: flex;
      flex-wrap: wrap;
      gap: 20px;
      margin-bottom: 30px;
      align-items: center;
    }

    .search-box, .filter-box {
      flex: 1;
      min-width: 250px;
    }

    .search-box input, .filter-box select, #auth-password-input {
      width: 100%;
      padding: 12px 16px;
      border: 1px solid #ced4da;
      border-radius: 8px;
      font-size: 16px;
      font-family: inherit;
      background: var(--white);
      color: var(--text-color);
      transition: border-color 0.2s, box-shadow 0.2s;
    }

    .search-box input:focus, .filter-box select:focus, #auth-password-input:focus {
      outline: none;
      border-color: var(--primary-blue);
      box-shadow: 0 0 0 3px rgba(0, 119, 182, 0.15);
    }

    .filter-box select {
      cursor: pointer;
    }

    .action-buttons {
      display: flex;
      gap: 10px;
    }

    /* Buttons */
    button {
      padding: 12px 20px;
      border: 2px solid var(--primary-blue);
      background: var(--primary-blue);
      color: var(--white);
      font-size: 16px;
      font-weight: 600;
      border-radius: 8px;
      cursor: pointer;
      font-family: inherit;
      transition: all 0.2s;
      box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
    }

    button:hover {
      background: var(--dark-blue);
      border-color: var(--dark-blue);
      transform: translateY(-2px);
      box-shadow: 0 6px 15px rgba(2, 62, 138, 0.25);
    }
    
    #print-btn {
        background: var(--white);
        color: var(--primary-blue);
    }
    
    #print-btn:hover {
        background: var(--border-light);
        color: var(--dark-blue);
        border-color: var(--dark-blue);
    }

    button:disabled {
      opacity: 0.6;
      cursor: not-allowed;
      transform: none;
      box-shadow: none;
    }

    /* Sort Controls */
    .sort-controls {
      display: flex;
      gap: 10px;
      margin-bottom: 24px;
    }

    .sort-btn {
      padding: 8px 16px;
      font-size: 14px;
      border: 1px solid var(--border-light);
      background: var(--white);
      color: var(--primary-blue);
      transition: background 0.2s, color 0.2s, border-color 0.2s;
    }
    
    .sort-btn:hover {
        transform: none;
        box-shadow: none;
        background: #f0f4f8;
    }

    .sort-btn.active {
      background: var(--primary-blue);
      color: var(--white);
      border-color: var(--primary-blue);
    }
    
    .sort-btn.active:hover {
        background: var(--dark-blue);
        color: var(--white);
        border-color: var(--dark-blue);
    }

    /* Loading and No Results */
    .loading {
      text-align: center;
      padding: 40px;
      font-size: 18px;
      color: var(--primary-blue);
      font-weight: 500;
    }

    .spinner {
      display: inline-block;
      width: 24px;
      height: 24px;
      border: 3px solid rgba(0, 119, 182, 0.2);
      border-top: 3px solid var(--primary-blue);
      border-radius: 50%;
      animation: spin 1s linear infinite;
      margin-right: 10px;
      vertical-align: middle;
    }

    @keyframes spin {
      0% { transform: rotate(0deg); }
      100% { transform: rotate(360deg); }
    }

    .doctors-list {
      display: grid;
      grid-template-columns: repeat(auto-fill, minmax(300px, 1fr));
      gap: 20px;
    }

    /* Doctor Card */
    .doctor-card {
      border: 1px solid var(--border-light);
      padding: 24px;
      border-radius: 12px;
      background: var(--white);
      box-shadow: 0 4px 12px rgba(0, 0, 0, 0.05);
      transition: all 0.3s cubic-bezier(0.25, 0.8, 0.25, 1);
    }

    .doctor-card:hover {
      box-shadow: 0 8px 25px rgba(0, 119, 182, 0.15);
      transform: translateY(-3px);
      border-color: var(--primary-blue);
    }

    .doctor-name {
      font-size: 20px;
      font-weight: 700;
      margin: 0 0 6px 0;
      color: var(--dark-blue);
      letter-spacing: -0.3px;
    }

    .doctor-specialty {
      font-size: 15px;
      color: var(--light-blue);
      margin: 0 0 16px 0;
      font-weight: 600;
    }

    .doctor-phone {
      font-size: 16px;
      font-weight: 500;
    }

    .doctor-phone a {
      color: var(--primary-blue);
      text-decoration: none;
      border-bottom: 1px dashed var(--primary-blue);
      transition: all 0.2s;
    }

    .doctor-phone a:hover {
      color: var(--dark-blue);
      border-bottom: 2px solid var(--dark-blue);
    }

    .no-results {
      text-align: center;
      padding: 60px;
      font-size: 18px;
      color: var(--light-blue);
      font-weight: 500;
    }

    /* --- AUTH SCREEN STYLES (განახლებული) --- */
    .auth-overlay {
        position: fixed;
        top: 0;
        left: 0;
        width: 100%;
        height: 100%;
        background: var(--white, #ffffff); /* თეთრი ფონი */
        display: flex;
        justify-content: center;
        align-items: center;
        z-index: 1000;
        transition: opacity 0.3s ease-in-out;
    }

    .auth-box {
        background: var(--white);
        padding: 40px;
        border-radius: 12px;
        box-shadow: 0 10px 30px rgba(0, 0, 0, 0.1); /* ღია ჩრდილი თეთრ ფონზე */
        max-width: 400px;
        width: 90%;
        text-align: center;
        border: 1px solid var(--border-light); /* თეთრ ფონზე უკეთ გამოჩენისთვის */
    }

    .auth-box h2 {
        color: var(--dark-blue);
        margin-top: 0;
        margin-bottom: 20px;
        font-size: 24px;
        font-weight: 700;
    }

    .auth-box button {
        width: 100%;
        margin-top: 20px;
    }

    #auth-error-message {
        color: var(--error-red);
        margin-top: 10px;
        font-size: 14px;
        font-weight: 600;
        min-height: 20px;
    }
    
    /* ლოგოს სტილი */
    .auth-logo {
        width: 100%;
        max-width: 300px; /* ლოგოს მაქსიმალური სიგანე */
        margin: 0 auto 30px auto; /* ცენტრში განთავსება და ქვემოთ სივრცე */
    }

    .auth-logo img {
        width: 100%;
        height: auto;
        display: block;
    }

    /* Print Styles */
    @media print {
      .auth-overlay {
        display: none !important;
      }
      .controls,
      .action-buttons,
      .sort-controls,
      button {
        display: none !important;
      }

      .container {
        max-width: 100%;
        padding: 0;
      }

      .doctor-card {
        break-inside: avoid;
        box-shadow: none !important;
        page-break-inside: avoid;
        border: 1px solid #ccc !important;
        margin-bottom: 15px;
      }

      body {
        background: #ffffff;
      }
    }

    /* Responsive adjustments */
    @media (max-width: 768px) {
      .container {
        padding: 20px 16px;
      }

      h1 {
        font-size: 28px;
      }
      
      .subtitle {
          font-size: 16px;
      }

      .controls {
        flex-direction: column;
        gap: 15px;
      }

      .search-box,
      .filter-box {
        width: 100%;
        min-width: 100%;
      }

      .action-buttons {
        width: 100%;
        justify-content: space-between;
      }
      
      .action-buttons button {
          flex: 1;
      }

      .sort-controls {
        flex-wrap: wrap;
        gap: 8px;
        justify-content: center;
      }
      
      .doctors-list {
          grid-template-columns: 1fr;
      }
      
      /* AUTH mobile adjustments */
      .auth-box {
        padding: 30px 20px; 
        width: 95%; 
      }
      
      .auth-logo {
        max-width: 250px; 
        margin-bottom: 20px;
      }
    }
  </style>
  <style>@view-transition { navigation: auto; }</style>
  <script src="/_sdk/data_sdk.js" type="text/javascript"></script>
  <script src="https://cdn.tailwindcss.com" type="text/javascript"></script>
 </head>
 <body>
    <div id="auth-overlay" class="auth-overlay">
        <div class="auth-box">
            <div class="auth-logo">
                <img src="./tm_center_logo.png" alt="TM Center კლინიკის ლოგო">
            </div>
            
            <h2 style="margin-top: 0;">ავტორიზაცია</h2>
            <p style="color: var(--dark-blue); font-size: 15px; margin-bottom: 25px;">შეიყვანეთ პაროლი</p>
            <input type="password" id="auth-password-input" placeholder="პაროლი" aria-label="Enter password">
            <div id="auth-error-message"></div>
            <button id="auth-login-btn">შესვლა</button>
        </div>
    </div>
    
    <div id="main-content" style="display: none;">
        <div class="container">
            <header>
                <h1 id="clinic-title">თბილისის სახელმწიფო სამედიცინო უნივერსტიტეტისა და ინგოროყვას მაღალი სამედიცინო ტექნოლოგიების საუნივერსიტეტო კლინიკა</h1>
                <p class="subtitle" id="clinic-subtitle">ექიმების სატელეფონო სია</p>
            </header>
            <div class="controls">
                <div class="search-box">
                    <input type="text" id="search-input" placeholder="ძებნა სახელით ან გვარით..." aria-label="Search doctors by name">
                </div>
                <div class="filter-box">
                    <label for="specialty-filter" style="display: none;">Filter by specialty</label>
                    <select id="specialty-filter" aria-label="Filter by specialty">
                        <option value="">ყველა სპეციალობა</option>
                    </select>
                </div>
                <div class="action-buttons">
                    <button id="refresh-btn" aria-label="Refresh data"><span id="refresh-text">განახლება</span></button>
                    <button id="print-btn" onclick="window.print()" aria-label="Print directory"><span id="print-text">ბეჭდვა</span></button>
                </div>
            </div>
            <div class="sort-controls">
                <button class="sort-btn active" id="sort-name" aria-label="Sort by name">A → Z (სახელი)</button>
                <button class="sort-btn" id="sort-specialty" aria-label="Sort by specialty">სპეციალობა</button>
            </div>
            <div id="loading" class="loading" style="display: none;"><span class="spinner"></span> მონაცემები იტვირთება...</div>
            <div id="doctors-list" class="doctors-list"></div>
            <div id="no-results" class="no-results" style="display: none;">
                შედეგები არ მოიძებნა
            </div>
        </div>
    </div>
  <script>
    // --- AUTHENTICATION CONFIGURATION ---
    const CORRECT_PASSWORD = "med123"; // <-- შეცვალეთ ეს პაროლით, რომელიც გსურთ!
    
    // --- ORIGINAL CONFIG & DATA ---
    const defaultConfig = {
      clinic_title: 'სამედიცინო დირექტორია',
      clinic_subtitle: 'ექიმების სატელეფონო სია',
      refresh_button_text: 'განახლება',
      print_button_text: 'ბეჭდვა',
      search_placeholder: 'ძებნა სახელით ან გვარით...',
      primary_color: '#0077b6', // Primary Blue
      background_color: '#f8f9fa', // Light Background
      text_color: '#023e8a', // Dark Blue
      secondary_text_color: '#00b4d8', // Light Blue/Accent
      border_color: '#0077b6',
      font_family: 'Inter',
      font_size: 16
    };

    let config = { ...defaultConfig };
    let allDoctors = [];
    let currentSort = 'name';

    const sampleDoctors = [
      { name: 'პაატა ბარათაშვილი', specialty: 'რადიოლოგი', phone: '593 311 748' },
      { name: 'ვაჟა თავბერიძე', specialty: 'რადიოლოგი', phone: '551 470 471' },
      { name: 'მაია', specialty: 'რენტგენი', phone: '557 654 351' },
      { name: 'ნინო', specialty: 'რენტგენი', phone: '599 400 311' },
      { name: 'ნაზი', specialty: 'რენტგენი', phone: '555 181 801' },
      { name: 'მარიამი', specialty: 'რენტგენი', phone: '598 100 644' },
      { name: 'მანანა გოგოლაძე', specialty: 'ექოსკოპია', phone: '577 450 049' },
      { name: 'ანა ინგოროყვა', specialty: 'ექოსკოპია', phone: '599 222 201' },
      { name: 'თამარ გოგელია', specialty: 'ექოსკოპია', phone: '557 424 363' },
      { name: 'მარიამ გავაშელი', specialty: 'ექოსკოპია', phone: '544 447 346' },
      { name: 'ბელა ანთიძე', specialty: 'ექოსკოპია', phone: '595 245 500' },
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
      { name: 'თამარი ტურიაშვილი', specialty: 'ინფექციური სნეულებები', phone: '598 005 186' },
      { name: 'ირინა კილაძე', specialty: 'ინფექციური სნეულებები', phone: '599 88 35 77' },
      { name: 'ეკატერინე მარკოზია', specialty: 'ინფექციური სნეულებები', phone: '555 739 633' },
      { name: 'ლაშა სარალიძე', specialty: 'ზოგადი ქირურგია', phone: '599 977 762' },
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

    // --- AUTHENTICATION LOGIC ---
    function handleLogin() {
        const passwordInput = document.getElementById('auth-password-input');
        const errorMessage = document.getElementById('auth-error-message');
        
        if (passwordInput.value === CORRECT_PASSWORD) {
            document.getElementById('auth-overlay').style.opacity = '0';
            setTimeout(() => {
                document.getElementById('auth-overlay').style.display = 'none';
                document.getElementById('main-content').style.display = 'block';
                fetchDoctors(); // Load data only after successful login
            }, 300);
        } else {
            errorMessage.textContent = 'არასწორი პაროლი! სცადეთ ხელახლა.';
            passwordInput.value = ''; // Clear input on failure
            passwordInput.focus();
        }
    }

    // Attach event listeners for login
    document.getElementById('auth-login-btn').addEventListener('click', handleLogin);
    document.getElementById('auth-password-input').addEventListener('keypress', (e) => {
        if (e.key === 'Enter') {
            handleLogin();
        }
    });

    // --- DIRECTORY FUNCTIONS (Unchanged from previous version, just moved) ---

    async function onConfigChange(newConfig) {
      config = { ...config, ...newConfig };
      
      const baseSize = config.font_size || defaultConfig.font_size;
      const customFont = config.font_family || defaultConfig.font_family;
      const baseFontStack = '-apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif';
      
      document.documentElement.style.setProperty('--primary-blue', config.primary_color || defaultConfig.primary_color);
      document.documentElement.style.setProperty('--dark-blue', config.text_color || defaultConfig.text_color);
      document.documentElement.style.setProperty('--light-blue', config.secondary_text_color || defaultConfig.secondary_text_color);
      document.documentElement.style.setProperty('--bg-light', config.background_color || defaultConfig.background_color);
      document.documentElement.style.setProperty('--border-color', config.border_color || defaultConfig.border_color);
      
      document.getElementById('clinic-title').textContent = config.clinic_title || defaultConfig.clinic_title;
      document.getElementById('clinic-title').style.fontFamily = `${customFont}, ${baseFontStack}`;
      document.getElementById('clinic-title').style.fontSize = `${baseSize * 2 + 4}px`;
      document.getElementById('clinic-title').style.color = config.text_color || defaultConfig.text_color;
      
      document.getElementById('clinic-subtitle').textContent = config.clinic_subtitle || defaultConfig.clinic_subtitle;
      document.getElementById('clinic-subtitle').style.fontFamily = `${customFont}, ${baseFontStack}`;
      document.getElementById('clinic-subtitle').style.fontSize = `${baseSize + 2}px`;
      document.getElementById('clinic-subtitle').style.color = config.secondary_text_color || defaultConfig.secondary_text_color;
      
      document.getElementById('refresh-text').textContent = config.refresh_button_text || defaultConfig.refresh_button_text;
      document.getElementById('print-text').textContent = config.print_button_text || defaultConfig.print_button_text;
      document.getElementById('search-input').placeholder = config.search_placeholder || defaultConfig.search_placeholder;
      
      document.body.style.fontFamily = `${customFont}, ${baseFontStack}`;
      document.body.style.fontSize = `${baseSize}px`;
      
      renderDoctors();
    }

    function mapToCapabilities(cfg) {
      return {
        recolorables: [
          {
            get: () => cfg.background_color || defaultConfig.background_color,
            set: (value) => {
              cfg.background_color = value;
              if (window.elementSdk) window.elementSdk.setConfig({ background_color: value });
            }
          },
          {
            get: () => cfg.border_color || defaultConfig.border_color,
            set: (value) => {
              cfg.border_color = value;
              if (window.elementSdk) window.elementSdk.setConfig({ border_color: value });
            }
          },
          {
            get: () => cfg.text_color || defaultConfig.text_color,
            set: (value) => {
              cfg.text_color = value;
              if (window.elementSdk) window.elementSdk.setConfig({ text_color: value });
            }
          },
          {
            get: () => cfg.secondary_text_color || defaultConfig.secondary_text_color,
            set: (value) => {
              cfg.secondary_text_color = value;
              if (window.elementSdk) window.elementSdk.setConfig({ secondary_text_color: value });
            }
          },
          {
            get: () => cfg.primary_color || defaultConfig.primary_color,
            set: (value) => {
              cfg.primary_color = value;
              if (window.elementSdk) window.elementSdk.setConfig({ primary_color: value });
            }
          }
        ],
        borderables: [],
        fontEditable: {
          get: () => cfg.font_family || defaultConfig.font_family,
          set: (value) => {
            cfg.font_family = value;
            if (window.elementSdk) window.elementSdk.setConfig({ font_family: value });
          }
        },
        fontSizeable: {
          get: () => cfg.font_size || defaultConfig.font_size,
          set: (value) => {
            cfg.font_size = value;
            if (window.elementSdk) window.elementSdk.setConfig({ font_size: value });
          }
        }
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

    async function fetchDoctors() {
      const loadingEl = document.getElementById('loading');
      const listEl = document.getElementById('doctors-list');
      const refreshBtn = document.getElementById('refresh-btn');
      
      loadingEl.style.display = 'block';
      listEl.style.display = 'none';
      refreshBtn.disabled = true;
      
      try {
        await new Promise(resolve => setTimeout(resolve, 800));
        allDoctors = [...sampleDoctors];
        
        populateSpecialtyFilter();
        renderDoctors();
      } catch (error) {
        console.error('Error fetching doctors:', error);
        allDoctors = [...sampleDoctors];
        populateSpecialtyFilter();
        renderDoctors();
      } finally {
        loadingEl.style.display = 'none';
        // Note: listEl display is set in renderDoctors based on results
        refreshBtn.disabled = false;
      }
    }

    function populateSpecialtyFilter() {
      const specialties = [...new Set(allDoctors.map(d => d.specialty))].sort((a, b) => a.localeCompare(b.name, 'ka'));
      const filterEl = document.getElementById('specialty-filter');
      
      const currentValue = filterEl.value;

      filterEl.innerHTML = '<option value="">ყველა სპეციალობა</option>';
      specialties.forEach(specialty => {
        const option = document.createElement('option');
        option.value = specialty;
        option.textContent = specialty;
        filterEl.appendChild(option);
      });
      
      if (currentValue && specialties.includes(currentValue)) {
          filterEl.value = currentValue;
      }
    }

    function renderDoctors() {
      const searchTerm = document.getElementById('search-input').value.toLowerCase();
      const specialtyFilter = document.getElementById('specialty-filter').value;
      
      let filtered = allDoctors.filter(doctor => {
        const matchesSearch = doctor.name.toLowerCase().includes(searchTerm);
        const matchesSpecialty = !specialtyFilter || doctor.specialty === specialtyFilter;
        return matchesSearch && matchesSpecialty;
      });
      
      if (currentSort === 'name') {
        filtered.sort((a, b) => a.name.localeCompare(b.name, 'ka'));
      } else {
        filtered.sort((a, b) => a.specialty.localeCompare(b.specialty, 'ka') || a.name.localeCompare(b.name, 'ka'));
      }
      
      const listEl = document.getElementById('doctors-list');
      const noResultsEl = document.getElementById('no-results');
      
      if (filtered.length === 0) {
        listEl.style.display = 'none';
        noResultsEl.style.display = 'block';
        return;
      }
      
      listEl.style.display = 'grid';
      noResultsEl.style.display = 'none';
      listEl.innerHTML = '';
      
      const baseSize = config.font_size || defaultConfig.font_size;
      const customFont = config.font_family || defaultConfig.font_family;
      const baseFontStack = '-apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, sans-serif';
      const textColor = config.text_color || defaultConfig.text_color;
      const secondaryTextColor = config.secondary_text_color || defaultConfig.secondary_text_color;
      const borderColor = config.border_color || defaultConfig.border_color;
      
      filtered.forEach(doctor => {
        const card = document.createElement('div');
        card.className = 'doctor-card';
        card.style.borderColor = borderColor;
        
        const name = document.createElement('div');
        name.className = 'doctor-name';
        name.textContent = doctor.name;
        name.style.fontFamily = `${customFont}, ${baseFontStack}`;
        name.style.fontSize = `${baseSize * 1.25}px`;
        name.style.color = textColor;
        
        const specialty = document.createElement('div');
        specialty.className = 'doctor-specialty';
        specialty.textContent = doctor.specialty;
        specialty.style.fontFamily = `${customFont}, ${baseFontStack}`;
        specialty.style.fontSize = `${baseSize - 1}px`; 
        specialty.style.color = secondaryTextColor;
        
        const phone = document.createElement('div');
        phone.className = 'doctor-phone';
        phone.style.fontFamily = `${customFont}, ${baseFontStack}`;
        phone.style.fontSize = `${baseSize}px`;
        
        const phoneLink = document.createElement('a');
        phoneLink.href = `tel:+995${doctor.phone.replace(/\s/g, '')}`;
        phoneLink.textContent = doctor.phone;
        phoneLink.style.color = borderColor; 
        phoneLink.style.borderBottomColor = borderColor;
        
        phone.appendChild(phoneLink);
        card.appendChild(name);
        card.appendChild(specialty);
        card.appendChild(phone);
        listEl.appendChild(card);
      });
    }

    // Event Listeners for Directory (Only activate after successful login)
    document.getElementById('search-input').addEventListener('input', renderDoctors);
    document.getElementById('specialty-filter').addEventListener('change', renderDoctors);
    
    document.getElementById('refresh-btn').addEventListener('click', fetchDoctors);
    
    document.getElementById('sort-name').addEventListener('click', () => {
      currentSort = 'name';
      document.getElementById('sort-name').classList.add('active');
      document.getElementById('sort-specialty').classList.remove('active');
      renderDoctors();
    });
    
    document.getElementById('sort-specialty').addEventListener('click', () => {
      currentSort = 'specialty';
      document.getElementById('sort-specialty').classList.add('active');
      document.getElementById('sort-name').classList.remove('active');
      renderDoctors();
    });

    // SDK Init
    if (window.elementSdk) {
      window.elementSdk.init({
        defaultConfig,
        onConfigChange,
        mapToCapabilities,
        mapToEditPanelValues
      });
    }
    
    // Initial Focus on Password Input (after document is ready)
    document.addEventListener('DOMContentLoaded', () => {
        document.getElementById('auth-password-input').focus();
    });
    
    // NOTE: fetchDoctors is called ONLY after successful login (in handleLogin function)
  </script>
 </body>
</html>
