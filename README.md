
<!DOCTYPE html>
<html lang="ar" dir="rtl">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>tikoverlay</title>
    <link rel="stylesheet" href="/dashboard/dashboard.css" />
    <link rel="stylesheet" href="/overlay/overlay-composer.css" />
    <link rel="icon" href="/tikoverlay.png" type="image/png" />
    <link rel="apple-touch-icon" href="/tikoverlay.png" />
    <meta property="og:image" content="/tikoverlay.png" />
    <meta name="twitter:image" content="/tikoverlay.png" />
    <meta name="msapplication-TileImage" content="/tikoverlay.png" />
    <link
      rel="stylesheet"
      href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css"
      crossorigin="anonymous"
      referrerpolicy="no-referrer"
    />
    <script src="https://unpkg.com/@supabase/supabase-js@2"></script>
    <script src="https://cdn.jsdelivr.net/npm/tus-js-client@2.3.0/dist/tus.min.js"></script>
    <script src="/socket.io/socket.io.js"></script>
  </head>

  <body>
    <!-- شاشة تحذير الجلسة المكررة -->
    <div
      id="sessionDuplicateOverlay"
      style="
        display: none;
        position: fixed;
        top: 0;
        left: 0;
        width: 100%;
        height: 100%;
        background: #1a1a2e;
        z-index: 99999;
        align-items: center;
        justify-content: center;
      "
    >
      <div style="text-align: center; padding: 40px">
        <div style="font-size: 80px; margin-bottom: 20px">🐙</div>
        <div
          style="
            background: rgba(255, 165, 0, 0.1);
            border: 2px solid rgba(255, 165, 0, 0.3);
            border-radius: 50%;
            width: 40px;
            height: 40px;
            display: inline-flex;
            align-items: center;
            justify-content: center;
            margin-bottom: 30px;
          "
        >
          <span style="color: #ffa500; font-size: 24px; font-weight: bold"
            >!</span
          >
        </div>
        <h1
          style="
            color: #e2e8f0;
            font-size: 32px;
            margin: 0 0 15px 0;
            font-weight: 600;
          "
        >
          Duplicate execution detected!
        </h1>
        <p
          style="
            color: #a0aec0;
            font-size: 16px;
            margin: 0 0 30px 0;
            max-width: 600px;
          "
        >
          It seems that Tikoverlay is already running in another browser tab or
          desktop app. Please locate and close the second instance!
        </p>
        <button
          id="takeOverSessionBtn"
          style="
            padding: 14px 32px;
            background: linear-gradient(135deg, #e53e3e 0%, #c53030 100%);
            border: none;
            border-radius: 8px;
            color: white;
            font-size: 16px;
            font-weight: 600;
            cursor: pointer;
            transition: all 0.3s ease;
            box-shadow: 0 4px 12px rgba(229, 62, 62, 0.3);
          "
          onmouseover="this.style.transform='translateY(-2px)'; this.style.boxShadow='0 6px 16px rgba(229, 62, 62, 0.4)';"
          onmouseout="this.style.transform='translateY(0)'; this.style.boxShadow='0 4px 12px rgba(229, 62, 62, 0.3)';"
        >
          Take over this session
        </button>
      </div>
    </div>

    <div id="viewport">
      <div class="dashboard-container">
        <!-- هيدر ثابت -->
        <header class="header">
          <div class="header-content">
            <h1>tikoverlay</h1>
            <div class="header-center">
              <div class="connection-status-header">
                <div class="status-indicator offline" id="connectionStatus">
                  <span class="status-dot"></span>
                  <span class="status-text">غير متصل</span>
                </div>
                <div class="connection-controls">
                  <input
                    type="text"
                    id="usernameInput"
                    placeholder="اسم المستخدم"
                    class="username-input-header"
                  />
                  <button id="connectBtn" class="connect-btn-header">
                    اتصال
                  </button>
                </div>
              </div>
            </div>
            <div class="user-info">
              <span id="userEmail">جاري التحميل...</span>
              <button id="logoutBtn" class="logout-btn">تسجيل الخروج</button>
            </div>
          </div>
        </header>

        <!-- القائمة الجانبية الثابتة -->
        <aside class="sidebar">
          <nav class="sidebar-nav">
            <ul class="nav-menu">
              <li>
                <a href="#" class="nav-item active" data-page="setup"
                  >التسجيل</a
                >
              </li>
              <li>
                <a href="#" class="nav-item" data-page="actions">الإجراءات</a>
              </li>
              <li>
                <a href="#" class="nav-item" data-page="follows">Overlays</a>
              </li>
              <li>
                <a href="#" class="nav-item" data-page="top-likes">
                  الدعم الاعجابات</a
                >
              </li>
              <li>
                <a href="#" class="nav-item" data-page="comments">OBS Docks</a>
              </li>

              <li>
                <a href="#" class="nav-item" data-page="goals">الأهداف</a>
              </li>
              <li>
                <a href="#" class="nav-item" data-page="chatbot">BOT CHAT</a>
              </li>
              <li><a href="#" class="nav-item" data-page="tts">TTS</a></li>
              <li>
                <a href="#" class="nav-item" data-page="social-rotator">
                  وسائل التواصل</a
                >
              </li>
              <li>
                <a href="#" class="nav-item" data-page="overlay-composer">
                  تجميع الروابط</a
                >
              </li>

              <li class="nav-divider"></li>
              <li>
                <a
                  href="terms-of-service.html"
                  class="nav-item nav-external"
                  target="_blank"
                  rel="noopener noreferrer"
                >
                  شروط الخدمة</a
                >
              </li>
              <li>
                <a
                  href="privacy-policy.html"
                  class="nav-item nav-external"
                  target="_blank"
                  rel="noopener noreferrer"
                >
                  سياسة الخصوصية</a
                >
              </li>
            </ul>
          </nav>
        </aside>

        <!-- الأزرار الثابتة أسفل القائمة الجانبية (خارجها) -->
        <a href="/app" class="sidebar-download-btn">تحميل التطبيق</a>
        <div class="sidebar-fixed-counter">
          <span class="counter-number"> الاصدار 1.2.2</span>
        </div>

        <!-- منطقة المحتوى -->
        <main class="main-content">
          <!-- صفحة Setup (تسجيل الدخول) -->
          <div id="setup-page" class="page active">
            <div class="page-header">
              <h2>تسجيل الدخول</h2>
            </div>

            <!-- تخطيط شبكي للصفحة -->
            <div class="setup-grid-layout">
              <!-- العمود الأيمن: نماذج تسجيل الدخول -->
              <div class="setup-right-column">
                <!-- نموذج تسجيل الدخول -->
                <div id="login-section" class="card">
                  <h3>مرحباً بك</h3>
                  <p class="login-subtitle">
                    قم بتسجيل الدخول للوصول إلى لوحة التحكم
                  </p>

                  <!-- زر تسجيل الدخول بواسطة Google -->
                  <div class="google-login-section">
                    <button
                      type="button"
                      id="googleLoginBtn"
                      class="google-login-btn"
                    >
                      <svg
                        class="google-icon"
                        width="18"
                        height="18"
                        viewBox="0 0 18 18"
                        xmlns="http://www.w3.org/2000/svg"
                      >
                        <g fill="none" fill-rule="evenodd">
                          <path
                            d="M17.64 9.205c0-.639-.057-1.252-.164-1.841H9v3.481h4.844a4.14 4.14 0 0 1-1.796 2.716v2.259h2.908c1.702-1.567 2.684-3.875 2.684-6.615z"
                            fill="#4285F4"
                          />
                          <path
                            d="M9 18c2.43 0 4.467-.806 5.956-2.18l-2.908-2.259c-.806.54-1.837.86-3.048.86-2.344 0-4.328-1.584-5.036-3.711H.957v2.332A8.997 8.997 0 0 0 9 18z"
                            fill="#34A853"
                          />
                          <path
                            d="M3.964 10.71A5.41 5.41 0 0 1 3.682 9c0-.593.102-1.17.282-1.71V4.958H.957A8.996 8.996 0 0 0 0 9c0 1.452.348 2.827.957 4.042l3.007-2.332z"
                            fill="#FBBC05"
                          />
                          <path
                            d="M9 3.58c1.321 0 2.508.454 3.44 1.345l2.582-2.58C13.463.891 11.426 0 9 0A8.997 8.997 0 0 0 .957 4.958L3.964 7.29C4.672 5.163 6.656 3.58 9 3.58z"
                            fill="#EA4335"
                          />
                        </g>
                      </svg>
                      <span>تسجيل الدخول باستخدام Google</span>
                    </button>
                  </div>

                  <!-- الفاصل -->
                  <div class="login-divider">
                    <span>أو</span>
                  </div>

                  <form id="loginForm" class="login-form">
                    <div class="form-group">
                      <label for="email">البريد الإلكتروني:</label>
                      <input
                        type="email"
                        id="email"
                        name="email"
                        required
                        placeholder="أدخل بريدك الإلكتروني"
                      />
                    </div>

                    <div
                      class="form-group"
                      id="codeGroup"
                      style="display: none"
                    >
                      <label for="verificationCode"
                        >رمز التحقق (6 أرقام):</label
                      >
                      <input
                        type="text"
                        id="verificationCode"
                        name="verificationCode"
                        maxlength="6"
                        pattern="[0-9]{6}"
                        placeholder="أدخل الرمز المرسل"
                      />

                      <!-- ملاحظة البريد الغير مرغوب فيه -->
                      <div class="email-notice">
                        <svg
                          width="16"
                          height="16"
                          viewBox="0 0 24 24"
                          fill="none"
                          stroke="currentColor"
                          stroke-width="2"
                        >
                          <circle cx="12" cy="12" r="10"></circle>
                          <line x1="12" y1="16" x2="12" y2="12"></line>
                          <line x1="12" y1="8" x2="12.01" y2="8"></line>
                        </svg>
                        <span
                          >لم يصلك الرمز؟ تأكد من صندوق البريد الغير مرغوب فيه
                          (Spam)</span
                        >
                      </div>

                      <!-- زر إعادة إرسال الرمز -->
                      <button
                        type="button"
                        id="resendCodeBtn"
                        class="resend-code-btn"
                      >
                        <svg
                          width="16"
                          height="16"
                          viewBox="0 0 24 24"
                          fill="none"
                          stroke="currentColor"
                          stroke-width="2"
                        >
                          <polyline points="23 4 23 10 17 10"></polyline>
                          <path d="M20.49 15a9 9 0 1 1-2.12-9.36L23 10"></path>
                        </svg>
                        <span class="resend-text">إعادة إرسال الرمز</span>
                      </button>
                    </div>

                    <button type="button" id="loginBtn" class="login-btn">
                      إرسال رمز التحقق
                    </button>

                    <div id="message" class="message"></div>
                  </form>

                  <div
                    id="loadingSpinner"
                    class="spinner"
                    style="display: none"
                  >
                    <div class="spinner-circle"></div>
                    <p>جاري التحميل...</p>
                  </div>
                </div>

                <!-- قسم المستخدم المسجل -->
                <div id="logged-in-section" class="card" style="display: none">
                  <div class="logged-in-header">
                    <h3>مرحباً بعودتك!</h3>
                  </div>
                  <div class="user-info-card">
                    <div class="user-info-row">
                      <div class="user-avatar-icon">
                        <svg
                          xmlns="http://www.w3.org/2000/svg"
                          width="24"
                          height="24"
                          viewBox="0 0 24 24"
                          fill="none"
                          stroke="currentColor"
                          stroke-width="2"
                          stroke-linecap="round"
                          stroke-linejoin="round"
                        >
                          <path
                            d="M20 21v-2a4 4 0 0 0-4-4H8a4 4 0 0 0-4 4v2"
                          ></path>
                          <circle cx="12" cy="7" r="4"></circle>
                        </svg>
                      </div>
                      <div class="user-info-text">
                        <div class="user-email-label">البريد الإلكتروني</div>
                        <div class="user-email-value" id="displayUserEmail">
                          جاري التحميل...
                        </div>
                      </div>
                    </div>
                    <div class="user-status-badge">
                      <svg
                        xmlns="http://www.w3.org/2000/svg"
                        width="14"
                        height="14"
                        viewBox="0 0 24 24"
                        fill="none"
                        stroke="currentColor"
                        stroke-width="2"
                        stroke-linecap="round"
                        stroke-linejoin="round"
                      >
                        <path d="M22 11.08V12a10 10 0 1 1-5.93-9.14"></path>
                        <polyline points="22 4 12 14.01 9 11.01"></polyline>
                      </svg>
                      <span>متصل بنجاح</span>
                    </div>
                  </div>
                  <button id="logoutBtnSetup" class="logout-btn-new">
                    <svg
                      xmlns="http://www.w3.org/2000/svg"
                      width="18"
                      height="18"
                      viewBox="0 0 24 24"
                      fill="none"
                      stroke="currentColor"
                      stroke-width="2"
                      stroke-linecap="round"
                      stroke-linejoin="round"
                    >
                      <path d="M9 21H5a2 2 0 0 1-2-2V5a2 2 0 0 1 2-2h4"></path>
                      <polyline points="16 17 21 12 16 7"></polyline>
                      <line x1="21" y1="12" x2="9" y2="12"></line>
                    </svg>
                    <span>تسجيل الخروج</span>
                  </button>
                  <div class="welcome-info-new">
                    <svg
                      xmlns="http://www.w3.org/2000/svg"
                      width="16"
                      height="16"
                      viewBox="0 0 24 24"
                      fill="none"
                      stroke="currentColor"
                      stroke-width="2"
                      stroke-linecap="round"
                      stroke-linejoin="round"
                    >
                      <circle cx="12" cy="12" r="10"></circle>
                      <line x1="12" y1="16" x2="12" y2="12"></line>
                      <line x1="12" y1="8" x2="12.01" y2="8"></line>
                    </svg>
                    <p>
                      يمكنك الآن الوصول إلى جميع ميزات لوحة التحكم من خلال
                      القائمة الجانبية.
                    </p>
                  </div>
                </div>
              </div>

              <!-- العمود الأيسر: قسم الاشتراك -->
              <div class="setup-left-column">
                <!-- 💳 قسم الاشتراك -->
                <div
                  id="subscription-section"
                  class="card"
                  style="display: none"
                >
                  <!-- حالة الاشتراك -->
                  <div id="subscription-status" class="subscription-status">
                    <!-- سيتم ملؤها بـ JavaScript -->
                  </div>

                  <!-- الباقات المتاحة (تظهر فقط للمستخدمين غير المشتركين) -->
                  <div
                    id="pricing-plans"
                    class="pricing-plans"
                    style="display: none"
                  >
                    <!-- صندوق الاشتراك الحديث -->
                    <div class="modern-subscription-box">
                      <!-- العنوان الرئيسي -->
                      <div class="subscription-box-header">
                        <div class="header-icon">
                          <svg
                            xmlns="http://www.w3.org/2000/svg"
                            width="28"
                            height="28"
                            viewBox="0 0 24 24"
                            fill="none"
                            stroke="currentColor"
                            stroke-width="2"
                            stroke-linecap="round"
                            stroke-linejoin="round"
                          >
                            <path d="M12 2L2 7l10 5 10-5-10-5z"></path>
                            <path d="M2 17l10 5 10-5"></path>
                            <path d="M2 12l10 5 10-5"></path>
                          </svg>
                        </div>
                        <h3>يجب عليك الاشتراك لاستخدام جميع الميزات</h3>
                      </div>

                      <!-- جدول الميزات -->
                      <div class="features-table">
                        <div class="feature-item">
                          <svg
                            class="check-icon"
                            xmlns="http://www.w3.org/2000/svg"
                            width="20"
                            height="20"
                            viewBox="0 0 24 24"
                            fill="none"
                            stroke="currentColor"
                            stroke-width="3"
                            stroke-linecap="round"
                            stroke-linejoin="round"
                          >
                            <polyline points="20 6 9 17 4 12"></polyline>
                          </svg>
                          <span>إشعارات فورية للمتابعات والإعجابات</span>
                        </div>
                        <div class="feature-item">
                          <svg
                            class="check-icon"
                            xmlns="http://www.w3.org/2000/svg"
                            width="20"
                            height="20"
                            viewBox="0 0 24 24"
                            fill="none"
                            stroke="currentColor"
                            stroke-width="3"
                            stroke-linecap="round"
                            stroke-linejoin="round"
                          >
                            <polyline points="20 6 9 17 4 12"></polyline>
                          </svg>
                          <span>عرض التعليقات على الشاشة مباشرة</span>
                        </div>
                        <div class="feature-item">
                          <svg
                            class="check-icon"
                            xmlns="http://www.w3.org/2000/svg"
                            width="20"
                            height="20"
                            viewBox="0 0 24 24"
                            fill="none"
                            stroke="currentColor"
                            stroke-width="3"
                            stroke-linecap="round"
                            stroke-linejoin="round"
                          >
                            <polyline points="20 6 9 17 4 12"></polyline>
                          </svg>
                          <span>إجراءات تلقائية للهدايا والأحداث</span>
                        </div>
                        <div class="feature-item">
                          <svg
                            class="check-icon"
                            xmlns="http://www.w3.org/2000/svg"
                            width="20"
                            height="20"
                            viewBox="0 0 24 24"
                            fill="none"
                            stroke="currentColor"
                            stroke-width="3"
                            stroke-linecap="round"
                            stroke-linejoin="round"
                          >
                            <polyline points="20 6 9 17 4 12"></polyline>
                          </svg>
                          <span>لوحة تحكم متقدمة للأهداف</span>
                        </div>
                        <div class="feature-item">
                          <svg
                            class="check-icon"
                            xmlns="http://www.w3.org/2000/svg"
                            width="20"
                            height="20"
                            viewBox="0 0 24 24"
                            fill="none"
                            stroke="currentColor"
                            stroke-width="3"
                            stroke-linecap="round"
                            stroke-linejoin="round"
                          >
                            <polyline points="20 6 9 17 4 12"></polyline>
                          </svg>
                          <span>ويدجت قائمة الداعمين والمتصدرين</span>
                        </div>
                        <div class="feature-item">
                          <svg
                            class="check-icon"
                            xmlns="http://www.w3.org/2000/svg"
                            width="20"
                            height="20"
                            viewBox="0 0 24 24"
                            fill="none"
                            stroke="currentColor"
                            stroke-width="3"
                            stroke-linecap="round"
                            stroke-linejoin="round"
                          >
                            <polyline points="20 6 9 17 4 12"></polyline>
                          </svg>
                          <span>تحديثات مستمرة ودعم فني متواصل</span>
                        </div>
                      </div>

                      <!-- قسم السعر والاشتراك -->
                      <div class="subscription-footer">
                        <!-- زر الاشتراك مباشرةً - بدون حقول خصم -->
                        <div class="subscribe-action">
                          <button
                            class="subscribe-btn-new"
                            data-plan="monthly"
                            id="subscribeNowBtn"
                          >
                            <svg
                              class="btn-icon"
                              xmlns="http://www.w3.org/2000/svg"
                              width="20"
                              height="20"
                              viewBox="0 0 24 24"
                              fill="none"
                              stroke="currentColor"
                              stroke-width="2"
                              stroke-linecap="round"
                              stroke-linejoin="round"
                            >
                              <circle cx="12" cy="12" r="10"></circle>
                              <polyline points="12 6 12 12 16 14"></polyline>
                            </svg>
                            <span>اشترك الآن</span>
                          </button>
                          <div class="price-display">
                            <span class="price-amount">$9.99</span>
                            <span class="price-period">/شهر</span>
                          </div>
                        </div>

                        <!-- ملاحظة الأمان -->
                        <div class="security-note">
                          <svg
                            xmlns="http://www.w3.org/2000/svg"
                            width="14"
                            height="14"
                            viewBox="0 0 24 24"
                            fill="none"
                            stroke="currentColor"
                            stroke-width="2"
                            stroke-linecap="round"
                            stroke-linejoin="round"
                          >
                            <rect
                              x="3"
                              y="11"
                              width="18"
                              height="11"
                              rx="2"
                              ry="2"
                            ></rect>
                            <path d="M7 11V7a5 5 0 0 1 10 0v4"></path>
                          </svg>
                          <span
                            >السعر والخصومات تُدار بواسطة Lemon Squeezy</span
                          >
                        </div>
                      </div>
                    </div>
                  </div>
                </div>
              </div>
            </div>
          </div>

          <!-- صفحة الإجراءات -->
          <div id="actions-page" class="page">
            <div class="page-header">
              <h2>الإجراءات</h2>
            </div>

            <!-- إنشاء إجراء للهدايا -->
            <div class="card">
              <div class="actions-header">
                <div class="actions-header-left">
                  <button id="addActionBtn" class="add-action-btn">
                    <span class="plus-icon">+</span>
                    <span>إضافة إجراء</span>
                  </button>
                </div>

                <div
                  id="lastCountryBox"
                  class="last-country-box"
                  style="display: none"
                  aria-live="polite"
                >
                  <div class="last-country-title">آخر تحقق من الدولة</div>
                  <div class="last-country-content">
                    <span class="lc-event" id="lcEvent">—</span>
                    <span class="lc-user" id="lcUser">جاري الانتظار...</span>
                  </div>
                  <span class="lc-country" id="lcCountry"></span>
                  <button
                    id="closeLastCountry"
                    class="last-country-close"
                    title="إغلاق"
                  >
                    ×
                  </button>
                </div>
              </div>

              <div id="actionsList"></div>
            </div>

            <!-- Overlay Screen Settings -->
            <div class="card">
              <h3>Overlay Screen Settings</h3>

              <div id="overlay-list"></div>
            </div>
          </div>

          <!-- نافذة إنشاء الإجراء المنبثقة -->
          <div id="actionPopup" class="popup-overlay" style="display: none">
            <div class="popup-container">
              <div class="popup-header">
                <h3 id="popupTitle">إنشاء إجراء جديد 🎁</h3>
                <button id="closePopupBtn" class="close-popup-btn">
                  &times;
                </button>
              </div>

              <div class="popup-content">
                <div class="form-section">
                  <label for="actName">اسم الإجراء:</label>
                  <input
                    id="actName"
                    type="text"
                    placeholder="اسم الإجراء (اختياري)"
                  />
                </div>

                <!-- قسم اختيار نوع الحدث -->
                <div class="form-section trigger-type-section">
                  <label class="section-title">نوع الحدث:</label>
                  <p class="section-note">
                    ⚠️ يمكنك اختيار نوع حدث واحد فقط لكل إجراء
                  </p>

                  <div class="trigger-options">
                    <!-- خيار 1: هدية محددة -->
                    <div class="trigger-option">
                      <label class="checkbox-label trigger-checkbox">
                        <input
                          type="checkbox"
                          id="triggerSpecificGift"
                          name="triggerType"
                          value="specific_gift"
                        />
                        <span class="checkmark"></span>
                        <span class="trigger-label">🎁 هدية محددة</span>
                      </label>

                      <div
                        id="specificGiftOptions"
                        class="trigger-sub-options"
                        style="display: none"
                      >
                        <div class="form-section">
                          <label>اختر الهدية:</label>
                          <div class="gift-selector-wrapper">
                            <div class="custom-select-wrapper">
                              <select
                                id="actGiftSelect"
                                class="gift-selector"
                                style="display: none"
                              >
                                <option value="">جاري تحميل الهدايا...</option>
                              </select>
                              <div
                                id="customGiftSelector"
                                class="custom-gift-selector"
                              >
                                <div class="selected-gift" id="selectedGift">
                                  <span>جاري تحميل الهدايا...</span>
                                </div>
                                <div class="gifts-dropdown" id="giftsDropdown">
                                  <!-- سيتم ملؤها بـ JavaScript -->
                                </div>
                              </div>
                            </div>
                            <button
                              id="refreshGiftsBtn"
                              class="refresh-gifts-btn"
                              title="تحديث قائمة الهدايا"
                            >
                              🔄
                            </button>
                          </div>
                        </div>

                        <!-- معاينة الهدية المحددة -->
                        <div
                          id="giftPreview"
                          class="gift-preview"
                          style="display: none"
                        >
                          <div class="gift-preview-content">
                            <img
                              id="giftPreviewImage"
                              src=""
                              alt="صورة الهدية"
                              onerror="this.style.display='none'"
                            />
                            <div class="gift-preview-info">
                              <div
                                id="giftPreviewName"
                                class="gift-preview-name"
                              ></div>
                              <div
                                id="giftPreviewDiamonds"
                                class="gift-preview-diamonds"
                              ></div>
                            </div>
                          </div>
                        </div>

                        <!-- خيار إظهار صورة الهدية (للهدية المحددة) -->
                        <div class="form-section">
                          <label class="checkbox-label">
                            <input
                              type="checkbox"
                              id="actShowGiftImageSpecific"
                            />
                            <span class="checkmark"></span>
                            إظهار صورة الهدية
                          </label>
                        </div>
                      </div>
                    </div>

                    <!-- خيار 2: حد أدنى للألماسات -->
                    <div class="trigger-option">
                      <label class="checkbox-label trigger-checkbox">
                        <input
                          type="checkbox"
                          id="triggerMinDiamonds"
                          name="triggerType"
                          value="min_diamonds"
                        />
                        <span class="checkmark"></span>
                        <span class="trigger-label">💎 حد أدنى للألماسات</span>
                      </label>

                      <div
                        id="minDiamondsOptions"
                        class="trigger-sub-options"
                        style="display: none"
                      >
                        <div class="form-section">
                          <label for="actMinValue">أدنى قيمة الألماسات:</label>
                          <input
                            id="actMinValue"
                            type="number"
                            min="1"
                            value="100"
                            placeholder="مثال: 100"
                          />
                          <small
                            >سيتم تفعيل الإجراء عند إرسال هدية بألماسات تساوي أو
                            تتجاوز هذه القيمة</small
                          >
                        </div>

                        <!-- خيار إظهار صورة الهدية (للحد الأدنى) -->
                        <div class="form-section">
                          <label class="checkbox-label">
                            <input
                              type="checkbox"
                              id="actShowGiftImageDiamonds"
                            />
                            <span class="checkmark"></span>
                            إظهار صورة الهدية
                          </label>
                        </div>
                      </div>
                    </div>

                    <!-- خيار 3: حد أدنى للايكات -->
                    <div class="trigger-option">
                      <label class="checkbox-label trigger-checkbox">
                        <input
                          type="checkbox"
                          id="triggerMinLikes"
                          name="triggerType"
                          value="min_likes"
                        />
                        <span class="checkmark"></span>
                        <span class="trigger-label">❤️ حد أدنى للايكات</span>
                      </label>

                      <div
                        id="minLikesOptions"
                        class="trigger-sub-options"
                        style="display: none"
                      >
                        <div class="form-section">
                          <label for="actMinLikes">أدنى عدد اللايكات:</label>
                          <input
                            id="actMinLikes"
                            type="number"
                            min="1"
                            value="50"
                            placeholder="مثال: 50"
                          />
                          <small
                            >سيتم تفعيل الإجراء عندما يصل عدد اللايكات إلى هذه
                            القيمة أو يتجاوزها، ثم يتم إعادة تعيين العداد</small
                          >
                        </div>
                      </div>
                    </div>

                    <div class="trigger-option">
                      <label class="checkbox-label trigger-checkbox">
                        <input
                          type="checkbox"
                          id="triggerFollow"
                          name="triggerType"
                          value="follow"
                        />
                        <span class="checkmark"></span>
                        <span class="trigger-label">👥 متابعة</span>
                      </label>

                      <div
                        id="followOptions"
                        class="trigger-sub-options"
                        style="display: none"
                      >
                        <div class="form-section">
                          <label>سيتم تفعيل الإجراء عند كل متابعة جديدة</label>
                          <small
                            >سيظهر اسم المتابع وصورته مع الفيديو والصوت (إن
                            وُجدا) على الشاشة المحددة</small
                          >
                        </div>
                      </div>
                    </div>

                    <div class="trigger-option">
                      <label class="checkbox-label trigger-checkbox">
                        <input
                          type="checkbox"
                          id="triggerJoin"
                          name="triggerType"
                          value="join"
                        />
                        <span class="checkmark"></span>
                        <span class="trigger-label">🚪 انضمام</span>
                      </label>

                      <div
                        id="joinOptions"
                        class="trigger-sub-options"
                        style="display: none"
                      >
                        <div class="form-section">
                          <label
                            >سيتم تفعيل الإجراء عند انضمام شخص جديد للبث</label
                          >
                          <small
                            >سيظهر اسم الشخص وصورته مع الفيديو والصوت (إن وُجدا)
                            على الشاشة المحددة</small
                          >
                        </div>
                      </div>
                    </div>

                    <div class="trigger-option">
                      <label class="checkbox-label trigger-checkbox">
                        <input
                          type="checkbox"
                          id="triggerSpecificUserJoin"
                          name="triggerType"
                          value="specific_user_join"
                        />
                        <span class="checkmark"></span>
                        <span class="trigger-label">👤 انضمام شخص معين</span>
                      </label>

                      <div
                        id="specificUserJoinOptions"
                        class="trigger-sub-options"
                        style="display: none"
                      >
                        <div class="form-section">
                          <label for="specificUsername"
                            >اسم المستخدم (Username)</label
                          >
                          <input
                            type="text"
                            id="specificUsername"
                            placeholder="مثال: @username أو username"
                            class="form-input"
                            style="
                              margin-top: 10px;
                              padding: 10px;
                              border: 1px solid #ddd;
                              border-radius: 5px;
                              width: 100%;
                            "
                          />

                          <label
                            for="specificUserExecutionLimit"
                            style="display: block; margin-top: 20px"
                            >عدد مرات التنفيذ المسموحة في كل جلسة</label
                          >
                          <input
                            type="number"
                            id="specificUserExecutionLimit"
                            min="1"
                            max="100"
                            value="1"
                            placeholder="1"
                            class="form-input"
                            style="
                              margin-top: 10px;
                              padding: 10px;
                              border: 1px solid #ddd;
                              border-radius: 5px;
                              width: 100%;
                              max-width: 150px;
                            "
                          />

                          <small
                            style="display: block; margin-top: 8px; color: #666"
                          >
                            🔢 يحدد كم مرة يمكن تنفيذ الإجراء في نفس الجلسة
                            (مثلاً: 5 = يعمل 5 مرات)
                          </small>
                        </div>
                      </div>
                    </div>

                    <div class="trigger-option">
                      <label class="checkbox-label trigger-checkbox">
                        <input
                          type="checkbox"
                          id="triggerSpecificComment"
                          name="triggerType"
                          value="specific_comment"
                        />
                        <span class="checkmark"></span>
                        <span class="trigger-label">💬 تعليق معين</span>
                      </label>

                      <div
                        id="specificCommentOptions"
                        class="trigger-sub-options"
                        style="display: none"
                      >
                        <div class="form-section">
                          <label for="commentText">نص التعليق</label>
                          <input
                            type="text"
                            id="commentText"
                            placeholder="مثال: مرحبا"
                            class="form-input"
                            style="
                              margin-top: 10px;
                              padding: 10px;
                              border: 1px solid #ddd;
                              border-radius: 5px;
                              width: 100%;
                            "
                          />

                          <!-- خيار "من شخص معين" -->
                          <div style="margin-top: 15px">
                            <label class="checkbox-label">
                              <input type="checkbox" id="specificCommentUser" />
                              <span class="checkmark"></span>
                              من شخص معين
                            </label>
                          </div>

                          <div
                            id="specificCommentUserOptions"
                            style="display: none; margin-top: 10px"
                          >
                            <label for="specificCommentUsername"
                              >اسم المستخدم (Username)</label
                            >
                            <input
                              type="text"
                              id="specificCommentUsername"
                              placeholder="مثال: @ahmed أو ahmed"
                              class="form-input"
                              style="
                                margin-top: 10px;
                                padding: 10px;
                                border: 1px solid #ddd;
                                border-radius: 5px;
                                width: 100%;
                              "
                            />
                          </div>

                          <small
                            style="display: block; margin-top: 8px; color: #666"
                          >
                            💬 سيتم تفعيل الإجراء عندما يعلق شخص بنفس هذا
                            النص<br />
                            🌍 يدعم فلتر الدولة - الأولوية للإجراء المخصص
                            للدولة<br />
                            👤 إذا حددت شخص معين - الإجراء يعمل فقط لهذا الشخص
                          </small>
                        </div>
                      </div>
                    </div>
                  </div>
                </div>

                <!-- قسم نوع الإجراء -->
                <div class="form-section action-type-section">
                  <div class="section-header">
                    <h4 class="section-title">
                      <i class="fas fa-sliders-h"></i>
                      نوع الإجراء
                    </h4>
                    <span class="section-badge">يمكنك اختيار أكثر من نوع</span>
                  </div>

                  <div class="action-type-options">
                    <!-- خيار 1: تشغيل صوت -->
                    <div class="action-type-card">
                      <label class="action-checkbox-label">
                        <input type="checkbox" id="actPlaySound" />
                        <span class="action-checkmark"></span>
                        <div class="action-label-content">
                          <span class="action-icon">🔊</span>
                          <span class="action-title">تشغيل صوت</span>
                        </div>
                      </label>
                      <div
                        id="soundUploadWrapper"
                        class="upload-wrapper"
                        style="display: none; margin-top: 12px"
                      >
                        <!-- أزرار اختيار مصدر الصوت -->
                        <div class="sound-source-buttons">
                          <button
                            type="button"
                            id="uploadSoundBtn"
                            class="sound-source-btn"
                          >
                            <i class="fas fa-upload"></i>
                            رفع ملف صوت
                          </button>
                          <button
                            type="button"
                            id="soundLibraryBtn"
                            class="sound-source-btn"
                          >
                            <i class="fas fa-book"></i>
                            مكتبة الأصوات
                          </button>
                        </div>

                        <!-- عرض الصوت المحدد (من أي مصدر) -->
                        <div
                          id="selectedSoundDisplay"
                          class="selected-sound-display"
                          style="display: none"
                        >
                          <div class="selected-sound-info">
                            <span class="sound-icon">🎵</span>
                            <div class="sound-details">
                              <span class="sound-name" id="selectedSoundName"
                                >لم يتم الاختيار</span
                              >
                              <span
                                class="sound-source"
                                id="selectedSoundSource"
                                >المصدر: --</span
                              >
                            </div>
                            <button
                              type="button"
                              class="delete-sound-btn"
                              id="deleteSelectedSoundBtn"
                              title="حذف الصوت"
                            >
                              <i class="fas fa-trash"></i>
                            </button>
                          </div>
                        </div>

                        <!-- حقل رفع الملف (مخفي) -->
                        <input
                          type="file"
                          id="actSoundFile"
                          accept="audio/mpeg,audio/mp3,audio/wav,audio/ogg,audio/webm,audio/*"
                          style="display: none"
                        />

                        <!-- شريط التقدم للرفع -->
                        <div
                          id="soundUploadProgress"
                          class="upload-progress-container"
                          style="display: none"
                        >
                          <div class="progress-bar-wrapper">
                            <div
                              id="soundProgressBar"
                              class="progress-bar-fill"
                              style="width: 0%"
                            ></div>
                          </div>
                          <div class="progress-text">
                            <span id="soundProgressPercent">0%</span>
                            <button
                              type="button"
                              id="cancelSoundUpload"
                              class="cancel-upload-btn"
                              title="إلغاء الرفع"
                            >
                              <i class="fas fa-times"></i>
                            </button>
                          </div>
                        </div>
                      </div>
                    </div>

                    <!-- خيار 2: تشغيل فيديو -->
                    <div class="action-type-card">
                      <label class="action-checkbox-label">
                        <input type="checkbox" id="actPlayVideo" />
                        <span class="action-checkmark"></span>
                        <div class="action-label-content">
                          <span class="action-icon">🎥</span>
                          <span class="action-title">تشغيل فيديو</span>
                        </div>
                      </label>
                      <div
                        id="videoUploadWrapper"
                        class="upload-wrapper"
                        style="display: none; margin-top: 12px"
                      >
                        <!-- زر رفع الفيديو -->
                        <button
                          type="button"
                          id="uploadVideoBtn"
                          class="sound-source-btn"
                          style="width: 100%"
                        >
                          <i class="fas fa-upload"></i>
                          رفع ملف فيديو
                        </button>

                        <!-- عرض الفيديو المحدد -->
                        <div
                          id="selectedVideoDisplay"
                          class="selected-sound-display"
                          style="display: none"
                        >
                          <div class="selected-sound-info">
                            <span class="sound-icon">🎬</span>
                            <div class="sound-details">
                              <span class="sound-name" id="selectedVideoName"
                                >لم يتم الاختيار</span
                              >
                              <span
                                class="sound-source"
                                id="selectedVideoSource"
                                >المصدر: ملف محلي</span
                              >
                            </div>
                            <button
                              type="button"
                              class="delete-sound-btn"
                              id="deleteVideoBtn"
                              title="حذف الفيديو"
                            >
                              <i class="fas fa-trash"></i>
                            </button>
                          </div>
                        </div>

                        <!-- حقل رفع الملف (مخفي) -->
                        <input
                          type="file"
                          id="actVideoFile"
                          accept="video/mp4,video/avi,video/mov,video/quicktime,video/x-msvideo,video/webm,video/*"
                          style="display: none"
                        />

                        <!-- شريط التقدم للرفع -->
                        <div
                          id="videoUploadProgress"
                          class="upload-progress-container"
                          style="display: none"
                        >
                          <div class="progress-bar-wrapper">
                            <div
                              id="videoProgressBar"
                              class="progress-bar-fill"
                              style="width: 0%"
                            ></div>
                          </div>
                          <div class="progress-text">
                            <span id="videoProgressPercent">0%</span>
                            <button
                              type="button"
                              id="cancelVideoUpload"
                              class="cancel-upload-btn"
                              title="إلغاء الرفع"
                            >
                              <i class="fas fa-times"></i>
                            </button>
                          </div>
                        </div>

                        <!-- عرض الملف المخزن (للتعديل) -->
                        <div
                          id="currentVideoFile"
                          class="selected-sound-display"
                          style="display: none"
                        >
                          <div class="selected-sound-info">
                            <span class="sound-icon">🎬</span>
                            <div class="sound-details">
                              <span class="sound-name" id="currentVideoFileName"
                                >لا يوجد ملف</span
                              >
                              <span class="sound-source"
                                >المصدر: ملف محفوظ</span
                              >
                            </div>
                            <button
                              type="button"
                              class="delete-sound-btn"
                              id="deleteCurrentVideoBtn"
                              title="حذف الفيديو المحفوظ"
                            >
                              <i class="fas fa-trash"></i>
                            </button>
                          </div>
                        </div>
                      </div>
                    </div>

                    <!-- خيار 3: رسالة شكر -->
                    <div class="action-type-card">
                      <label class="action-checkbox-label">
                        <input type="checkbox" id="actShowThankYou" />
                        <span class="action-checkmark"></span>
                        <div class="action-label-content">
                          <span class="action-icon">💌</span>
                          <span class="action-title">رسالة</span>
                        </div>
                      </label>

                      <div
                        id="thankYouMessageOptions"
                        class="action-sub-options"
                        style="display: none"
                      >
                        <div class="message-input-group">
                          <label for="actThankYouMessage" class="message-label">
                            <i class="fas fa-comment-dots"></i>
                            نص الرسالة
                          </label>
                          <textarea
                            id="actThankYouMessage"
                            rows="3"
                            placeholder="مثال: شكراً على دعمك! ❤️"
                            maxlength="200"
                            class="message-textarea"
                          ></textarea>
                          <div class="message-footer">
                            <span class="char-count">
                              <span id="charCount">0</span> / 200 حرف
                            </span>
                          </div>
                        </div>
                      </div>
                    </div>

                    <!-- 🌍 خيار 4: فلترة حسب الدولة (NEW) -->
                    <div class="action-type-card">
                      <label class="action-checkbox-label">
                        <input type="checkbox" id="actCountryFilter" />
                        <span class="action-checkmark"></span>
                        <div class="action-label-content">
                          <span class="action-icon">🌍</span>
                          <span class="action-title">فلتر الدولة</span>
                        </div>
                      </label>

                      <div
                        id="countryFilterOptions"
                        class="action-sub-options"
                        style="display: none"
                      >
                        <div class="form-section">
                          <label for="actCountrySelect" class="message-label">
                            <i class="fas fa-globe"></i>
                            اختر الدول المسموحة
                          </label>

                          <!-- 🔍 مربع البحث -->
                          <div style="margin-bottom: 10px">
                            <input
                              type="text"
                              id="countrySearchBox"
                              placeholder="🔍 ابحث عن دولة..."
                              style="
                                width: 100%;
                                padding: 8px 12px;
                                border: 1px solid #ddd;
                                border-radius: 6px;
                                font-size: 14px;
                              "
                            />
                          </div>

                          <div class="country-selector-wrapper">
                            <select
                              id="actCountrySelect"
                              multiple
                              class="country-selector"
                            >
                              <optgroup label="🌍 الدول العربية">
                                <option value="JO">🇯🇴 الأردن</option>
                                <option value="AE">🇦🇪 الإمارات</option>
                                <option value="BH">🇧🇭 البحرين</option>
                                <option value="DZ">🇩🇿 الجزائر</option>
                                <option value="SD">🇸🇩 السودان</option>
                                <option value="SY">🇸🇾 سوريا</option>
                                <option value="SO">🇸🇴 الصومال</option>
                                <option value="IQ">🇮🇶 العراق</option>
                                <option value="OM">🇴🇲 عمان</option>
                                <option value="PS">🇵🇸 فلسطين</option>
                                <option value="QA">🇶🇦 قطر</option>
                                <option value="KM">🇰🇲 جزر القمر</option>
                                <option value="KW">🇰🇼 الكويت</option>
                                <option value="LB">🇱🇧 لبنان</option>
                                <option value="LY">🇱🇾 ليبيا</option>
                                <option value="EG">🇪🇬 مصر</option>
                                <option value="MA">🇲🇦 المغرب</option>
                                <option value="MR">🇲🇷 موريتانيا</option>
                                <option value="SA">🇸🇦 السعودية</option>
                                <option value="TN">🇹🇳 تونس</option>
                                <option value="YE">🇾🇪 اليمن</option>
                                <option value="DJ">🇩🇯 جيبوتي</option>
                              </optgroup>
                              <optgroup label="🌎 دول العالم">
                                <option value="AF">🇦🇫 أفغانستان</option>
                                <option value="AL">🇦🇱 ألبانيا</option>
                                <option value="DE">🇩🇪 ألمانيا</option>
                                <option value="AD">🇦🇩 أندورا</option>
                                <option value="AO">🇦🇴 أنغولا</option>
                                <option value="AG">🇦🇬 أنتيغوا وباربودا</option>
                                <option value="AR">🇦🇷 الأرجنتين</option>
                                <option value="AM">🇦🇲 أرمينيا</option>
                                <option value="AU">🇦🇺 أستراليا</option>
                                <option value="AT">🇦🇹 النمسا</option>
                                <option value="AZ">🇦🇿 أذربيجان</option>
                                <option value="BS">🇧🇸 جزر البهاما</option>
                                <option value="BD">🇧🇩 بنغلاديش</option>
                                <option value="BB">🇧🇧 باربادوس</option>
                                <option value="BY">🇧🇾 بيلاروسيا</option>
                                <option value="BE">🇧🇪 بلجيكا</option>
                                <option value="BZ">🇧🇿 بليز</option>
                                <option value="BJ">🇧🇯 بنين</option>
                                <option value="BT">🇧🇹 بوتان</option>
                                <option value="BO">🇧🇴 بوليفيا</option>
                                <option value="BA">🇧🇦 البوسنة والهرسك</option>
                                <option value="BW">🇧🇼 بوتسوانا</option>
                                <option value="BR">🇧🇷 البرازيل</option>
                                <option value="BN">🇧🇳 بروناي</option>
                                <option value="BG">🇧🇬 بلغاريا</option>
                                <option value="BF">🇧🇫 بوركينا فاسو</option>
                                <option value="BI">🇧🇮 بوروندي</option>
                                <option value="CV">🇨🇻 الرأس الأخضر</option>
                                <option value="KH">🇰🇭 كمبوديا</option>
                                <option value="CM">🇨🇲 الكاميرون</option>
                                <option value="CA">🇨🇦 كندا</option>
                                <option value="CF">
                                  🇨🇫 جمهورية أفريقيا الوسطى
                                </option>
                                <option value="TD">🇹🇩 تشاد</option>
                                <option value="CL">🇨🇱 تشيلي</option>
                                <option value="CN">🇨🇳 الصين</option>
                                <option value="CO">🇨🇴 كولومبيا</option>
                                <option value="CR">🇨🇷 كوستاريكا</option>
                                <option value="HR">🇭🇷 كرواتيا</option>
                                <option value="CU">🇨🇺 كوبا</option>
                                <option value="CY">🇨🇾 قبرص</option>
                                <option value="CZ">🇨🇿 التشيك</option>
                                <option value="CD">
                                  🇨🇩 الكونغو الديمقراطية
                                </option>
                                <option value="DK">🇩🇰 الدنمارك</option>
                                <option value="DM">🇩🇲 دومينيكا</option>
                                <option value="DO">
                                  🇩🇴 جمهورية الدومينيكان
                                </option>
                                <option value="EC">🇪🇨 الإكوادور</option>
                                <option value="SV">🇸🇻 السلفادور</option>
                                <option value="GQ">🇬🇶 غينيا الاستوائية</option>
                                <option value="ER">🇪🇷 إريتريا</option>
                                <option value="EE">🇪🇪 إستونيا</option>
                                <option value="SZ">🇸🇿 إسواتيني</option>
                                <option value="ET">🇪🇹 إثيوبيا</option>
                                <option value="FJ">🇫🇯 فيجي</option>
                                <option value="FI">🇫🇮 فنلندا</option>
                                <option value="FR">🇫🇷 فرنسا</option>
                                <option value="GA">🇬🇦 الغابون</option>
                                <option value="GM">🇬🇲 غامبيا</option>
                                <option value="GE">🇬🇪 جورجيا</option>
                                <option value="GH">🇬🇭 غانا</option>
                                <option value="GR">🇬🇷 اليونان</option>
                                <option value="GD">🇬🇩 غرينادا</option>
                                <option value="GT">🇬🇹 غواتيمالا</option>
                                <option value="GN">🇬🇳 غينيا</option>
                                <option value="GW">🇬🇼 غينيا بيساو</option>
                                <option value="GY">🇬🇾 غيانا</option>
                                <option value="HT">🇭🇹 هايتي</option>
                                <option value="HN">🇭🇳 هندوراس</option>
                                <option value="HU">🇭🇺 المجر</option>
                                <option value="IS">🇮🇸 آيسلندا</option>
                                <option value="IN">🇮🇳 الهند</option>
                                <option value="ID">🇮🇩 إندونيسيا</option>
                                <option value="IR">🇮🇷 إيران</option>
                                <option value="IE">🇮🇪 أيرلندا</option>
                                <option value="IL">🇮🇱 إسرائيل</option>
                                <option value="IT">🇮🇹 إيطاليا</option>
                                <option value="CI">🇨🇮 ساحل العاج</option>
                                <option value="JM">🇯🇲 جامايكا</option>
                                <option value="JP">🇯🇵 اليابان</option>
                                <option value="KZ">🇰🇿 كازاخستان</option>
                                <option value="KE">🇰🇪 كينيا</option>
                                <option value="KI">🇰🇮 كيريباتي</option>
                                <option value="KP">🇰🇵 كوريا الشمالية</option>
                                <option value="KR">🇰🇷 كوريا الجنوبية</option>
                                <option value="XK">🇽🇰 كوسوفو</option>
                                <option value="KG">🇰🇬 قيرغيزستان</option>
                                <option value="LA">🇱🇦 لاوس</option>
                                <option value="LV">🇱🇻 لاتفيا</option>
                                <option value="LS">🇱🇸 ليسوتو</option>
                                <option value="LR">🇱🇷 ليبيريا</option>
                                <option value="LI">🇱🇮 ليختنشتاين</option>
                                <option value="LT">🇱🇹 ليتوانيا</option>
                                <option value="LU">🇱🇺 لوكسمبورغ</option>
                                <option value="MG">🇲🇬 مدغشقر</option>
                                <option value="MW">🇲🇼 مالاوي</option>
                                <option value="MY">🇲🇾 ماليزيا</option>
                                <option value="MV">🇲🇻 المالديف</option>
                                <option value="ML">🇲🇱 مالي</option>
                                <option value="MT">🇲🇹 مالطا</option>
                                <option value="MH">🇲🇭 جزر مارشال</option>
                                <option value="MU">🇲🇺 موريشيوس</option>
                                <option value="MX">🇲🇽 المكسيك</option>
                                <option value="FM">🇫🇲 ميكرونيزيا</option>
                                <option value="MD">🇲🇩 مولدوفا</option>
                                <option value="MC">🇲🇨 موناكو</option>
                                <option value="MN">🇲🇳 منغوليا</option>
                                <option value="ME">🇲🇪 الجبل الأسود</option>
                                <option value="MZ">🇲🇿 موزمبيق</option>
                                <option value="MM">🇲🇲 ميانمار</option>
                                <option value="NA">🇳🇦 ناميبيا</option>
                                <option value="NR">🇳🇷 ناورو</option>
                                <option value="NP">🇳🇵 نيبال</option>
                                <option value="NL">🇳🇱 هولندا</option>
                                <option value="NZ">🇳🇿 نيوزيلندا</option>
                                <option value="NI">🇳🇮 نيكاراغوا</option>
                                <option value="NE">🇳🇪 النيجر</option>
                                <option value="NG">🇳🇬 نيجيريا</option>
                                <option value="MK">🇲🇰 مقدونيا الشمالية</option>
                                <option value="NO">🇳🇴 النرويج</option>
                                <option value="PK">🇵🇰 باكستان</option>
                                <option value="PW">🇵🇼 بالاو</option>
                                <option value="PA">🇵🇦 بنما</option>
                                <option value="PG">
                                  🇵🇬 بابوا غينيا الجديدة
                                </option>
                                <option value="PY">🇵🇾 باراغواي</option>
                                <option value="PE">🇵🇪 بيرو</option>
                                <option value="PH">🇵🇭 الفلبين</option>
                                <option value="PL">🇵🇱 بولندا</option>
                                <option value="PT">🇵🇹 البرتغال</option>
                                <option value="RO">🇷🇴 رومانيا</option>
                                <option value="RU">🇷🇺 روسيا</option>
                                <option value="RW">🇷🇼 رواندا</option>
                                <option value="KN">🇰🇳 سانت كيتس ونيفيس</option>
                                <option value="LC">🇱🇨 سانت لوسيا</option>
                                <option value="VC">
                                  🇻🇨 سانت فينسنت والغرينادين
                                </option>
                                <option value="WS">🇼🇸 ساموا</option>
                                <option value="SM">🇸🇲 سان مارينو</option>
                                <option value="ST">
                                  🇸🇹 ساو تومي وبرينسيبي
                                </option>
                                <option value="SN">🇸🇳 السنغال</option>
                                <option value="RS">🇷🇸 صربيا</option>
                                <option value="SC">🇸🇨 سيشل</option>
                                <option value="SL">🇸🇱 سيراليون</option>
                                <option value="SG">🇸🇬 سنغافورة</option>
                                <option value="SK">🇸🇰 سلوفاكيا</option>
                                <option value="SI">🇸🇮 سلوفينيا</option>
                                <option value="SB">🇸🇧 جزر سليمان</option>
                                <option value="ZA">🇿🇦 جنوب أفريقيا</option>
                                <option value="SS">🇸🇸 جنوب السودان</option>
                                <option value="ES">🇪🇸 إسبانيا</option>
                                <option value="LK">🇱🇰 سريلانكا</option>
                                <option value="SR">🇸🇷 سورينام</option>
                                <option value="SE">🇸🇪 السويد</option>
                                <option value="CH">🇨🇭 سويسرا</option>
                                <option value="TJ">🇹🇯 طاجيكستان</option>
                                <option value="TZ">🇹🇿 تنزانيا</option>
                                <option value="TH">🇹🇭 تايلاند</option>
                                <option value="TL">🇹🇱 تيمور الشرقية</option>
                                <option value="TG">🇹🇬 توغو</option>
                                <option value="TO">🇹🇴 تونغا</option>
                                <option value="TT">🇹🇹 ترينيداد وتوباغو</option>
                                <option value="TR">🇹🇷 تركيا</option>
                                <option value="TM">🇹🇲 تركمانستان</option>
                                <option value="TV">🇹🇻 توفالو</option>
                                <option value="UG">🇺🇬 أوغندا</option>
                                <option value="UA">🇺🇦 أوكرانيا</option>
                                <option value="GB">🇬🇧 المملكة المتحدة</option>
                                <option value="US">🇺🇸 الولايات المتحدة</option>
                                <option value="UY">🇺🇾 أوروغواي</option>
                                <option value="UZ">🇺🇿 أوزبكستان</option>
                                <option value="VU">🇻🇺 فانواتو</option>
                                <option value="VA">🇻🇦 الفاتيكان</option>
                                <option value="VE">🇻🇪 فنزويلا</option>
                                <option value="VN">🇻🇳 فيتنام</option>
                                <option value="ZM">🇿🇲 زامبيا</option>
                                <option value="ZW">🇿🇼 زيمبابوي</option>
                              </optgroup>
                            </select>
                            <div
                              class="selected-countries-display"
                              id="selectedCountriesDisplay"
                            >
                              <span class="placeholder-text"
                                >لم يتم اختيار دول (الإجراء يعمل لجميع
                                الدول)</span
                              >
                            </div>
                          </div>
                          <small style="display: block; margin-top: 8px">
                            💡 <strong>ملاحظة:</strong> الإجراء سيعمل فقط
                            للمستخدمين من الدول المختارة. إذا لم تختر أي دولة،
                            سيعمل الإجراء لجميع الدول.
                          </small>
                        </div>
                      </div>
                    </div>
                  </div>
                </div>

                <div class="form-row">
                  <div class="form-section">
                    <label for="actDurationSec">مدة الإشعار (ثواني):</label>
                    <input
                      id="actDurationSec"
                      type="number"
                      min="1"
                      value="10"
                    />
                  </div>
                </div>

                <div class="form-section">
                  <label for="actTargetScreen">الشاشة المستهدفة:</label>
                  <select id="actTargetScreen">
                    <option value="1">Screen 1</option>
                    <option value="2">Screen 2</option>
                    <option value="3">Screen 3</option>
                    <option value="4">Screen 4</option>
                    <option value="5">Screen 5</option>
                  </select>
                </div>

                <!-- التحكم بمستوى الصوت -->
                <div class="form-section">
                  <label for="actVolume">
                    <span>🔊 مستوى الصوت:</span>
                    <span
                      id="volumeValue"
                      style="color: #58a6ff; font-weight: bold"
                      >100%</span
                    >
                  </label>
                  <div class="volume-slider-container">
                    <input
                      type="range"
                      id="actVolume"
                      min="0"
                      max="100"
                      value="100"
                      class="volume-slider"
                    />
                    <div class="volume-markers">
                      <span>0%</span>
                      <span>50%</span>
                      <span>100%</span>
                    </div>
                  </div>
                  <small
                    >حدد مستوى صوت الإجراء (الصوت/الفيديو) على شاشة العرض</small
                  >
                </div>
              </div>

              <div class="popup-actions">
                <button id="advancedSettingsBtn" class="advanced-settings-btn">
                  <i class="fas fa-cog"></i>
                  إعدادات متقدمة
                </button>
                <button id="createActionBtn" class="create-action-btn">
                  Create Action
                </button>
                <button id="cancelActionBtn" class="cancel-action-btn">
                  إلغاء
                </button>
              </div>
            </div>
          </div>

          <!-- صفحة المتابعات والإعجابات المدمجة -->
          <div id="follows-page" class="page">
            <div class="page-header">
              <h2>Overlays</h2>
            </div>

            <!-- Container للعمودين -->
            <div class="podium-widgets-grid">
              <!-- عمود المتابعات -->
              <div class="podium-widget-column">
                <div class="card">
                  <div class="connection-info">
                    <div class="info-item">
                      <label>رابط المتابعات:</label>
                      <div class="url-controls-container">
                        <button
                          class="test-btn"
                          onclick="testFollowWidget()"
                          title="اختبار المتابعة"
                        >
                          <i class="fas fa-play"></i>
                        </button>
                        <button
                          class="settings-btn"
                          onclick="openFollowsSettings()"
                          title="إعدادات المتابعات"
                        >
                          <i class="fas fa-cog"></i>
                        </button>
                        <div class="code-display">
                          <span id="followsWidgetUrl">جاري التحميل...</span>
                          <button
                            class="copy-btn"
                            onclick="copyToClipboard('followsWidgetUrl')"
                          >
                            نسخ
                          </button>
                        </div>
                      </div>
                    </div>
                  </div>
                </div>
                <div class="card">
                  <div class="widget-preview-container">
                    <iframe
                      id="followsWidgetPreview"
                      src=""
                      frameborder="0"
                      allow="autoplay"
                      style="
                        width: 100%;
                        height: 500px;
                        background: transparent;
                        border-radius: 8px;
                      "
                    ></iframe>
                  </div>
                </div>
              </div>

              <!-- عمود الإعجابات -->
              <div class="podium-widget-column">
                <div class="card">
                  <div class="connection-info">
                    <div class="info-item">
                      <label>رابط الإعجابات:</label>
                      <div class="url-controls-container">
                        <button
                          class="test-btn"
                          onclick="testLikeWidget()"
                          title="اختبار الإعجاب"
                        >
                          <i class="fas fa-play"></i>
                        </button>
                        <button
                          class="settings-btn"
                          onclick="openLikesSettings()"
                          title="إعدادات الإعجابات"
                        >
                          <i class="fas fa-cog"></i>
                        </button>
                        <div class="code-display">
                          <span id="likesWidgetUrl">جاري التحميل...</span>
                          <button
                            class="copy-btn"
                            onclick="copyToClipboard('likesWidgetUrl')"
                          >
                            نسخ
                          </button>
                        </div>
                      </div>
                    </div>
                  </div>
                </div>
                <div class="card">
                  <div class="widget-preview-container">
                    <iframe
                      id="likesWidgetPreview"
                      src=""
                      frameborder="0"
                      allow="autoplay"
                      style="
                        width: 100%;
                        height: 500px;
                        background: transparent;
                        border-radius: 8px;
                      "
                    ></iframe>
                  </div>
                </div>
              </div>

              <!-- عمود قلوب اللايكات -->
              <div class="podium-widget-column">
                <div class="card">
                  <div class="connection-info">
                    <div class="info-item">
                      <label>رابط قلوب اللايكات:</label>
                      <div class="url-controls-container">
                        <div class="code-display">
                          <span id="likeHeartsWidgetUrl">جاري التحميل...</span>
                          <button
                            class="copy-btn"
                            onclick="copyToClipboard('likeHeartsWidgetUrl')"
                          >
                            نسخ
                          </button>
                        </div>
                      </div>
                    </div>
                  </div>
                </div>
                <div class="card">
                  <div class="widget-preview-container">
                    <iframe
                      id="likeHeartsWidgetPreview"
                      src=""
                      frameborder="0"
                      allow="autoplay"
                      style="
                        width: 100%;
                        height: 500px;
                        background: transparent;
                        border-radius: 8px;
                      "
                    ></iframe>
                  </div>
                </div>
              </div>

              <!-- عمود الألعاب النارية -->
              <div class="podium-widget-column">
                <div class="card">
                  <div class="connection-info">
                    <div class="info-item">
                      <label>رابط الألعاب النارية:</label>
                      <div class="url-controls-container">
                        <button
                          class="test-btn"
                          onclick="testFireworkWidget()"
                          title="اختبار الألعاب النارية"
                        >
                          <i class="fas fa-play"></i>
                        </button>
                        <button
                          class="settings-btn"
                          onclick="openFireworkSettings()"
                          title="إعدادات الألعاب النارية"
                        >
                          <i class="fas fa-cog"></i>
                        </button>
                        <div class="code-display">
                          <span id="fireworkWidgetUrl">جاري التحميل...</span>
                          <button
                            class="copy-btn"
                            onclick="copyToClipboard('fireworkWidgetUrl')"
                          >
                            نسخ
                          </button>
                        </div>
                      </div>
                    </div>
                  </div>
                </div>
                <div class="card">
                  <div class="widget-preview-container">
                    <iframe
                      id="fireworkWidgetPreview"
                      src=""
                      frameborder="0"
                      allow="autoplay"
                      style="width: 100%; height: 500px; border-radius: 8px"
                    ></iframe>
                  </div>
                </div>
              </div>
            </div>
          </div>

          <!-- صفحة أفضل المتفاعلين -->
          <div id="top-likes-page" class="page">
            <div class="page-header">
              <h2>أفضل المتفاعلين والداعمين</h2>
            </div>

            <!-- Container للعمودين -->
            <div class="podium-widgets-grid">
              <!-- عمود المتفاعلين -->
              <div class="podium-widget-column">
                <div class="card">
                  <div class="connection-info">
                    <div class="info-item">
                      <label>رابط أفضل المتفاعلين:</label>
                      <div class="url-controls-container">
                        <button
                          class="reset-likes-btn"
                          onclick="resetAllLikes()"
                        >
                          Reset
                        </button>
                        <button
                          class="settings-btn"
                          onclick="openPodiumSettings()"
                          title="إعدادات العرض"
                        >
                          <i class="fas fa-cog"></i>
                        </button>
                        <div class="code-display">
                          <span id="horizontalPodiumUrl">جاري التحميل...</span>
                          <button
                            class="copy-btn"
                            onclick="copyToClipboard('horizontalPodiumUrl')"
                          >
                            نسخ
                          </button>
                        </div>
                      </div>
                    </div>
                  </div>
                </div>
                <div class="card">
                  <div class="widget-preview-container">
                    <iframe
                      id="horizontalPodiumPreview"
                      src=""
                      frameborder="0"
                      allow="autoplay"
                      style="
                        width: 100%;
                        height: 500px;
                        background: transparent;
                        border-radius: 8px;
                      "
                    ></iframe>
                  </div>
                </div>
              </div>

              <!-- عمود الداعمين -->
              <div class="podium-widget-column">
                <div class="card">
                  <div class="connection-info">
                    <div class="info-item">
                      <label>رابط أفضل الداعمين:</label>
                      <div class="url-controls-container">
                        <button
                          class="reset-likes-btn"
                          onclick="resetAllSupporters()"
                        >
                          Reset
                        </button>
                        <button
                          class="settings-btn"
                          onclick="openSupportersSettings()"
                          title="إعدادات العرض"
                        >
                          <i class="fas fa-cog"></i>
                        </button>
                        <div class="code-display">
                          <span id="horizontalPodiumSupportersUrl"
                            >جاري التحميل...</span
                          >
                          <button
                            class="copy-btn"
                            onclick="copyToClipboard('horizontalPodiumSupportersUrl')"
                          >
                            نسخ
                          </button>
                        </div>
                      </div>
                    </div>
                  </div>
                </div>
                <div class="card">
                  <div class="widget-preview-container">
                    <iframe
                      id="horizontalPodiumSupportersPreview"
                      src=""
                      frameborder="0"
                      allow="autoplay"
                      style="
                        width: 100%;
                        height: 500px;
                        background: transparent;
                        border-radius: 8px;
                      "
                    ></iframe>
                  </div>
                </div>
              </div>
            </div>
          </div>

          <!-- صفحة التعليقات -->
          <div id="comments-page" class="page">
            <div class="page-header">
              <h2>OBS Docks</h2>
            </div>
            <div class="card">
              <div class="connection-info">
                <div class="info-item">
                  <label>رابط OBS Docks:</label>
                  <div class="code-display">
                    <span id="commentsWidgetUrl">جاري التحميل...</span>
                    <button
                      class="copy-btn"
                      onclick="copyToClipboard('commentsWidgetUrl')"
                    >
                      نسخ
                    </button>
                  </div>
                </div>
              </div>
            </div>
            <div class="card">
              <div class="widget-preview-container">
                <iframe
                  id="commentsWidgetPreview"
                  src=""
                  frameborder="0"
                  allow="autoplay"
                  style="
                    width: 100%;
                    height: 600px;
                    background: transparent;
                    border-radius: 8px;
                  "
                ></iframe>
              </div>
            </div>
          </div>

          <!-- صفحة بوت شات -->
          <div id="chatbot-page" class="page">
            <style></style>

            <div class="chatbot-header">
              <h2>🤖 بوت الشات التلقائي</h2>
              <p>إدارة الردود التلقائية الذكية على أحداث TikTok</p>
            </div>

            <!-- قسم تسجيل الدخول -->
            <div class="bot-login-card">
              <h3>
                <svg
                  width="18"
                  height="18"
                  viewBox="0 0 24 24"
                  fill="none"
                  stroke="currentColor"
                  stroke-width="2"
                >
                  <rect
                    x="3"
                    y="11"
                    width="18"
                    height="11"
                    rx="2"
                    ry="2"
                  ></rect>
                  <path d="M7 11V7a5 5 0 0 1 10 0v4"></path>
                </svg>
                تسجيل الدخول إلى TikTok
              </h3>

              <button id="tiktokLoginBtn" class="bot-login-btn">
                <svg
                  width="20"
                  height="20"
                  viewBox="0 0 24 24"
                  fill="none"
                  stroke="currentColor"
                  stroke-width="2"
                >
                  <rect
                    x="3"
                    y="11"
                    width="18"
                    height="11"
                    rx="2"
                    ry="2"
                  ></rect>
                  <path d="M7 11V7a5 5 0 0 1 10 0v4"></path>
                </svg>
                <span>تسجيل الدخول</span>
              </button>

              <div
                id="tiktokLoginStatus"
                class="login-status-success"
                style="display: none"
              >
                <svg
                  width="16"
                  height="16"
                  viewBox="0 0 24 24"
                  fill="none"
                  stroke="currentColor"
                  stroke-width="2"
                >
                  <path d="M22 11.08V12a10 10 0 1 1-5.93-9.14"></path>
                  <polyline points="22 4 12 14.01 9 11.01"></polyline>
                </svg>
                <span>تم إرسال طلب تسجيل الدخول بنجاح</span>
              </div>
            </div>

            <!-- قسم إدارة الإجراءات -->
            <div class="bot-actions-card">
              <div class="bot-actions-header">
                <h3>
                  <svg
                    width="20"
                    height="20"
                    viewBox="0 0 24 24"
                    fill="none"
                    stroke="currentColor"
                    stroke-width="2"
                  >
                    <path
                      d="M21 15a2 2 0 0 1-2 2H7l-4 4V5a2 2 0 0 1 2-2h14a2 2 0 0 1 2 2z"
                    ></path>
                  </svg>
                  إجراءات البوت
                </h3>
                <button id="addBotActionBtn" class="add-bot-action-btn">
                  <svg
                    width="18"
                    height="18"
                    viewBox="0 0 24 24"
                    fill="none"
                    stroke="currentColor"
                    stroke-width="2"
                  >
                    <line x1="12" y1="5" x2="12" y2="19"></line>
                    <line x1="5" y1="12" x2="19" y2="12"></line>
                  </svg>
                  <span>إضافة إجراء جديد</span>
                </button>
              </div>

              <div id="botActionsList" class="bot-actions-list">
                <!-- سيتم ملء الإجراءات هنا -->
              </div>

              <div
                id="botActionsEmptyState"
                class="bot-empty-state"
                style="display: none"
              >
                <h3>🤖 لا توجد إجراءات بعد</h3>
                <p>ابدأ بإنشاء إجراء تلقائي ذكي للرد على متابعيك</p>
              </div>
            </div>
          </div>

          <!-- نافذة إنشاء إجراء البوت المنبثقة -->
          <div id="botActionPopup" class="popup-overlay" style="display: none">
            <div class="popup-container">
              <div class="popup-header">
                <h3 id="botActionPopupTitle">إنشاء إجراء بوت جديد 🤖</h3>
                <button id="closeBotActionPopupBtn" class="close-popup-btn">
                  &times;
                </button>
              </div>

              <div class="popup-content">
                <!-- قسم اختيار نوع الحدث -->
                <div class="form-section trigger-type-section">
                  <label class="section-title">نوع الحدث:</label>
                  <p class="section-note">
                    ⚠️ يمكنك اختيار نوع حدث واحد فقط لكل إجراء
                  </p>

                  <div class="trigger-options">
                    <!-- خيار 1: هدية محددة -->
                    <div class="trigger-option">
                      <label class="checkbox-label trigger-checkbox">
                        <input
                          type="checkbox"
                          id="botTriggerSpecificGift"
                          name="botTriggerType"
                          value="specific_gift"
                        />
                        <span class="checkmark"></span>
                        <span class="trigger-label">🎁 هدية محددة</span>
                      </label>

                      <div
                        id="botSpecificGiftOptions"
                        class="trigger-sub-options"
                        style="display: none"
                      >
                        <div class="form-section">
                          <label>اختر الهدية:</label>
                          <div class="gift-selector-wrapper">
                            <div class="custom-select-wrapper">
                              <select
                                id="botActGiftSelect"
                                class="gift-selector"
                                style="display: none"
                              >
                                <option value="">جاري تحميل الهدايا...</option>
                              </select>
                              <div
                                id="botCustomGiftSelector"
                                class="custom-gift-selector"
                              >
                                <div class="selected-gift" id="botSelectedGift">
                                  <span>جاري تحميل الهدايا...</span>
                                </div>
                                <div
                                  class="gifts-dropdown"
                                  id="botGiftsDropdown"
                                >
                                  <!-- سيتم ملؤها بـ JavaScript -->
                                </div>
                              </div>
                            </div>
                            <button
                              id="botRefreshGiftsBtn"
                              class="refresh-gifts-btn"
                              title="تحديث قائمة الهدايا"
                            >
                              🔄
                            </button>
                          </div>
                        </div>

                        <!-- معاينة الهدية المحددة -->
                        <div
                          id="botGiftPreview"
                          class="gift-preview"
                          style="display: none"
                        >
                          <div class="gift-preview-content">
                            <img
                              id="botGiftPreviewImage"
                              src=""
                              alt="صورة الهدية"
                              onerror="this.style.display='none'"
                            />
                            <div class="gift-preview-info">
                              <div
                                id="botGiftPreviewName"
                                class="gift-preview-name"
                              ></div>
                              <div
                                id="botGiftPreviewDiamonds"
                                class="gift-preview-diamonds"
                              ></div>
                            </div>
                          </div>
                        </div>
                      </div>
                    </div>

                    <!-- خيار 2: حد أدنى للألماسات -->
                    <div class="trigger-option">
                      <label class="checkbox-label trigger-checkbox">
                        <input
                          type="checkbox"
                          id="botTriggerMinDiamonds"
                          name="botTriggerType"
                          value="min_diamonds"
                        />
                        <span class="checkmark"></span>
                        <span class="trigger-label">💎 حد أدنى للألماسات</span>
                      </label>

                      <div
                        id="botMinDiamondsOptions"
                        class="trigger-sub-options"
                        style="display: none"
                      >
                        <div class="form-section">
                          <label for="botActMinValue"
                            >أدنى قيمة الألماسات:</label
                          >
                          <input
                            id="botActMinValue"
                            type="number"
                            min="1"
                            value="100"
                            placeholder="مثال: 100"
                          />
                          <small
                            >سيتم تفعيل الإجراء عند إرسال هدية بألماسات تساوي أو
                            تتجاوز هذه القيمة</small
                          >
                        </div>
                      </div>
                    </div>

                    <!-- خيار 3: تعليق بكلمة مفتاحية -->
                    <div class="trigger-option">
                      <label class="checkbox-label trigger-checkbox">
                        <input
                          type="checkbox"
                          id="botTriggerComment"
                          name="botTriggerType"
                          value="comment"
                        />
                        <span class="checkmark"></span>
                        <span class="trigger-label"
                          >💬 تعليق بكلمة مفتاحية</span
                        >
                      </label>

                      <div
                        id="botCommentOptions"
                        class="trigger-sub-options"
                        style="display: none"
                      >
                        <div class="form-section">
                          <label for="botCommentKeyword"
                            >الكلمة المفتاحية:</label
                          >
                          <input
                            id="botCommentKeyword"
                            type="text"
                            placeholder="مثال: السلام عليكم"
                          />
                          <small
                            >سيتم تفعيل الإجراء عند وجود هذه الكلمة في
                            التعليق</small
                          >
                        </div>
                      </div>
                    </div>

                    <!-- خيار 4: متابعة -->
                    <div class="trigger-option">
                      <label class="checkbox-label trigger-checkbox">
                        <input
                          type="checkbox"
                          id="botTriggerFollow"
                          name="botTriggerType"
                          value="follow"
                        />
                        <span class="checkmark"></span>
                        <span class="trigger-label">👥 متابعة</span>
                      </label>

                      <div
                        id="botFollowOptions"
                        class="trigger-sub-options"
                        style="display: none"
                      >
                        <div class="form-section">
                          <label>سيتم تفعيل الإجراء عند كل متابعة جديدة</label>
                          <small>سيتم إرسال رسالة الرد تلقائياً في الشات</small>
                        </div>
                      </div>
                    </div>

                    <!-- خيار 5: انضمام -->
                    <div class="trigger-option">
                      <label class="checkbox-label trigger-checkbox">
                        <input
                          type="checkbox"
                          id="botTriggerJoin"
                          name="botTriggerType"
                          value="join"
                        />
                        <span class="checkmark"></span>
                        <span class="trigger-label">🚪 انضمام</span>
                      </label>

                      <div
                        id="botJoinOptions"
                        class="trigger-sub-options"
                        style="display: none"
                      >
                        <div class="form-section">
                          <label
                            >سيتم تفعيل الإجراء عند انضمام شخص جديد للبث</label
                          >
                          <small>سيتم إرسال رسالة الرد تلقائياً في الشات</small>
                        </div>
                      </div>
                    </div>
                  </div>
                </div>

                <!-- رسالة الرد -->
                <div class="form-section" style="margin-top: 20px">
                  <label for="botResponseMessage">
                    <svg
                      width="16"
                      height="16"
                      viewBox="0 0 24 24"
                      fill="none"
                      stroke="currentColor"
                      stroke-width="2"
                      style="
                        display: inline-block;
                        vertical-align: middle;
                        margin-left: 5px;
                      "
                    >
                      <path
                        d="M21 15a2 2 0 0 1-2 2H7l-4 4V5a2 2 0 0 1 2-2h14a2 2 0 0 1 2 2z"
                      ></path>
                    </svg>
                    رسالة الرد:
                  </label>
                  <textarea
                    id="botResponseMessage"
                    rows="4"
                    required
                    placeholder="مثال: شكراً على المتابعة ❤️"
                    style="
                      background: rgba(13, 27, 42, 0.6);
                      border: 1px solid rgba(102, 126, 234, 0.3);
                      border-radius: 10px;
                      padding: 14px;
                      color: #e2e8f0;
                      font-size: 14px;
                      resize: vertical;
                      transition: all 0.3s ease;
                    "
                    onfocus="this.style.borderColor='rgba(102, 126, 234, 0.6)'; this.style.boxShadow='0 0 0 3px rgba(102, 126, 234, 0.1)';"
                    onblur="this.style.borderColor='rgba(102, 126, 234, 0.3)'; this.style.boxShadow='none';"
                  ></textarea>
                  <small
                    style="
                      color: #a0aec0;
                      font-size: 12px;
                      display: flex;
                      align-items: center;
                      gap: 5px;
                      margin-top: 8px;
                    "
                  >
                    <svg
                      width="14"
                      height="14"
                      viewBox="0 0 24 24"
                      fill="none"
                      stroke="currentColor"
                      stroke-width="2"
                    >
                      <circle cx="12" cy="12" r="10"></circle>
                      <line x1="12" y1="16" x2="12" y2="12"></line>
                      <line x1="12" y1="8" x2="12.01" y2="8"></line>
                    </svg>
                    <span
                      >سيتم إضافة اسم المستخدم تلقائياً في بداية الرسالة</span
                    >
                  </small>
                </div>
              </div>

              <div class="popup-actions">
                <button id="saveBotActionBtn" class="create-action-btn">
                  حفظ الإجراء
                </button>
                <button id="cancelBotActionBtn" class="cancel-action-btn">
                  إلغاء
                </button>
              </div>
            </div>
          </div>

          <!-- صفحة الأهداف -->
          <div id="goals-page" class="page">
            <div class="page-header">
              <h2>الأهداف</h2>
            </div>

            <!-- Container للعمودين -->
            <div class="podium-widgets-grid">
              <!-- عمود هدف Heart Me -->
              <div class="podium-widget-column">
                <div class="card">
                  <div class="connection-info">
                    <div class="info-item">
                      <label>رابط هدف Heart Me:</label>
                      <div class="url-controls-container">
                        <button
                          class="reset-likes-btn"
                          onclick="resetHeartMeGoal()"
                          title="إعادة تعيين"
                        >
                          Reset
                        </button>
                        <button
                          class="test-btn"
                          onclick="testHeartMeGoal()"
                          title="اختبار الهدف"
                        >
                          <i class="fas fa-play"></i>
                        </button>
                        <button
                          class="settings-btn"
                          onclick="openGoalSettings()"
                          title="إعدادات الهدف"
                        >
                          <i class="fas fa-cog"></i>
                        </button>
                        <div class="code-display">
                          <span id="heartMeGoalUrl">جاري التحميل...</span>
                          <button
                            class="copy-btn"
                            onclick="copyToClipboard('heartMeGoalUrl')"
                          >
                            نسخ
                          </button>
                        </div>
                      </div>
                    </div>
                  </div>
                </div>
                <div class="card">
                  <div class="widget-preview-container">
                    <iframe
                      id="heartMeGoalPreview"
                      src=""
                      frameborder="0"
                      allow="autoplay"
                      style="
                        width: 100%;
                        height: 500px;
                        background: transparent;
                        border-radius: 8px;
                      "
                    ></iframe>
                  </div>
                </div>
              </div>

              <!-- عمود هدف اللايكات (Like Goal) -->
              <div class="podium-widget-column">
                <div class="card">
                  <div class="connection-info">
                    <div class="info-item">
                      <label>رابط هدف اللايكات (Like Goal):</label>
                      <div class="url-controls-container">
                        <button
                          class="reset-likes-btn"
                          onclick="resetLikeGoal()"
                          title="إعادة تعيين"
                        >
                          Reset
                        </button>
                        <button
                          class="test-btn"
                          onclick="testLikeGoal()"
                          title="اختبار الهدف"
                        >
                          <i class="fas fa-play"></i>
                        </button>
                        <button
                          class="settings-btn"
                          onclick="openLikeGoalSettings()"
                          title="إعدادات الهدف"
                        >
                          <i class="fas fa-cog"></i>
                        </button>
                        <div class="code-display">
                          <span id="likeGoalUrl">جاري التحميل...</span>
                          <button
                            class="copy-btn"
                            onclick="copyToClipboard('likeGoalUrl')"
                          >
                            نسخ
                          </button>
                        </div>
                      </div>
                    </div>
                  </div>
                </div>
                <div class="card">
                  <div class="widget-preview-container">
                    <iframe
                      id="likeGoalPreview"
                      src=""
                      frameborder="0"
                      allow="autoplay"
                      style="
                        width: 100%;
                        height: 500px;
                        background: transparent;
                        border-radius: 8px;
                      "
                    ></iframe>
                  </div>
                </div>
              </div>

              <!-- عمود هدف المتابعة -->
              <div class="podium-widget-column">
                <div class="card">
                  <div class="connection-info">
                    <div class="info-item">
                      <label>رابط هدف المتابعة:</label>
                      <div class="url-controls-container">
                        <button
                          class="reset-likes-btn"
                          onclick="resetFollowGoal()"
                          title="إعادة تعيين"
                        >
                          Reset
                        </button>
                        <button
                          class="test-btn"
                          onclick="testFollowGoal()"
                          title="اختبار الهدف"
                        >
                          <i class="fas fa-play"></i>
                        </button>
                        <button
                          class="settings-btn"
                          onclick="openFollowGoalSettings()"
                          title="إعدادات الهدف"
                        >
                          <i class="fas fa-cog"></i>
                        </button>
                        <div class="code-display">
                          <span id="followGoalUrl">جاري التحميل...</span>
                          <button
                            class="copy-btn"
                            onclick="copyToClipboard('followGoalUrl')"
                          >
                            نسخ
                          </button>
                        </div>
                      </div>
                    </div>
                  </div>
                </div>
                <div class="card">
                  <div class="widget-preview-container">
                    <iframe
                      id="followGoalPreview"
                      src=""
                      frameborder="0"
                      allow="autoplay"
                      style="
                        width: 100%;
                        height: 500px;
                        background: transparent;
                        border-radius: 8px;
                      "
                    ></iframe>
                  </div>
                </div>
              </div>

              <!-- عمود هدف المشاركة -->
              <div class="podium-widget-column">
                <div class="card">
                  <div class="connection-info">
                    <div class="info-item">
                      <label>رابط هدف المشاركة:</label>
                      <div class="url-controls-container">
                        <button
                          class="reset-likes-btn"
                          onclick="resetShareGoal()"
                          title="إعادة تعيين"
                        >
                          Reset
                        </button>
                        <button
                          class="test-btn"
                          onclick="testShareGoal()"
                          title="اختبار الهدف"
                        >
                          <i class="fas fa-play"></i>
                        </button>
                        <button
                          class="settings-btn"
                          onclick="openShareGoalSettings()"
                          title="إعدادات الهدف"
                        >
                          <i class="fas fa-cog"></i>
                        </button>
                        <div class="code-display">
                          <span id="shareGoalUrl">جاري التحميل...</span>
                          <button
                            class="copy-btn"
                            onclick="copyToClipboard('shareGoalUrl')"
                          >
                            نسخ
                          </button>
                        </div>
                      </div>
                    </div>
                  </div>
                </div>
                <div class="card">
                  <div class="widget-preview-container">
                    <iframe
                      id="shareGoalPreview"
                      src=""
                      frameborder="0"
                      allow="autoplay"
                      style="
                        width: 100%;
                        height: 500px;
                        background: transparent;
                        border-radius: 8px;
                      "
                    ></iframe>
                  </div>
                </div>
              </div>
            </div>
          </div>

          <!-- صفحة قارئ التعليقات الصوتي (TTS) -->
          <div id="tts-page" class="page">
            <div class="page-header">
              <h2>🔊 قارئ التعليقات الصوتي</h2>
            </div>

            <!-- رابط صفحة TTS (موجود فوق إعدادات الصفحة) -->
            <div class="card tts-link-card">
              <div class="connection-info">
                <div class="info-item">
                  <label>رابط قارئ التعليقات:</label>
                  <div class="url-controls-container">
                    <div class="code-display">
                      <span id="ttsUrl">جاري التحميل...</span>
                      <button
                        class="copy-btn"
                        onclick="copyToClipboard('ttsUrl')"
                      >
                        نسخ
                      </button>
                    </div>
                  </div>
                  <p class="setting-description">
                    استخدم هذا الرابط في برنامج البث (OBS) كمصدر متصفح لعرض
                    وتشغيل التعليقات الصوتية
                  </p>
                </div>
              </div>
            </div>

            <!-- إعدادات TTS -->
            <div class="card">
              <div class="tts-settings-grid">
                <div class="tts-column">
                  <!-- تفعيل قارئ التعليقات -->
                  <div class="tts-setting-card tts-enable-card">
                    <label class="tts-checkbox-label">
                      <input
                        type="checkbox"
                        id="ttsEnabled"
                        onchange="localStorage.setItem('tts_enabled', this.checked); saveTTSSettings()"
                      />
                      <span class="tts-checkmark"></span>
                      <div class="tts-label-content">
                        <span class="tts-icon">🔊</span>
                        <div class="tts-label-text">
                          <span class="tts-title"
                            >تفعيل قارئ التعليقات الصوتي</span
                          >
                        </div>
                      </div>
                    </label>
                  </div>

                  <!-- تصفية التعليقات -->
                  <div class="tts-setting-card">
                    <div class="tts-setting-header">
                      <span class="tts-icon">🎯</span>
                      <span class="tts-setting-title">تصفية التعليقات</span>
                    </div>
                    <div class="tts-filter-checkboxes">
                      <!-- جميع التعليقات -->
                      <label class="tts-filter-checkbox-label">
                        <input
                          type="checkbox"
                          id="ttsFilterAll"
                          checked
                          onchange="handleTTSFilterChange()"
                        />
                        <span class="tts-filter-checkmark"></span>
                        <span class="tts-filter-text">📝 جميع التعليقات</span>
                      </label>

                      <!-- المشرفين -->
                      <label class="tts-filter-checkbox-label">
                        <input
                          type="checkbox"
                          id="ttsFilterModerators"
                          onchange="handleTTSFilterChange()"
                        />
                        <span class="tts-filter-checkmark"></span>
                        <span class="tts-filter-text">🛡️ المشرفين فقط</span>
                      </label>

                      <!-- المشتركين -->
                      <label class="tts-filter-checkbox-label">
                        <input
                          type="checkbox"
                          id="ttsFilterSubscribers"
                          onchange="handleTTSFilterChange()"
                        />
                        <span class="tts-filter-checkmark"></span>
                        <span class="tts-filter-text">⭐ المشتركين فقط</span>
                      </label>

                      <!-- المتابعون -->
                      <label class="tts-filter-checkbox-label">
                        <input
                          type="checkbox"
                          id="ttsFilterFollowers"
                          onchange="handleTTSFilterChange()"
                        />
                        <span class="tts-filter-checkmark"></span>
                        <span class="tts-filter-text">❤️ المتابعون فقط</span>
                      </label>

                      <!-- مستخدمين محددين -->
                      <label class="tts-filter-checkbox-label">
                        <input
                          type="checkbox"
                          id="ttsFilterSpecific"
                          onchange="handleTTSFilterChange()"
                        />
                        <span class="tts-filter-checkmark"></span>
                        <span class="tts-filter-text">👥 مستخدمين محددين</span>
                      </label>
                    </div>
                  </div>

                  <!-- أسماء المستخدمين المسموح لهم -->
                  <div
                    class="tts-setting-card"
                    id="allowedUsersContainer"
                    style="display: none"
                  >
                    <div class="tts-setting-header">
                      <span class="tts-icon">👥</span>
                      <span class="tts-setting-title"
                        >المستخدمون المسموح لهم</span
                      >
                    </div>
                    <div class="tts-usernames-wrapper">
                      <textarea
                        id="ttsAllowedUsernames"
                        rows="6"
                        class="tts-textarea"
                        placeholder="اكتب اسم مستخدم في كل سطر&#10;مثال:&#10;ahmad&#10;mohamed&#10;ali"
                        onchange="localStorage.setItem('tts_allowed_usernames', this.value)"
                      ></textarea>
                      <button class="tts-save-btn" onclick="saveTTSSettings()">
                        <svg
                          xmlns="http://www.w3.org/2000/svg"
                          width="16"
                          height="16"
                          viewBox="0 0 24 24"
                          fill="none"
                          stroke="currentColor"
                          stroke-width="2"
                          stroke-linecap="round"
                          stroke-linejoin="round"
                        >
                          <path
                            d="M19 21H5a2 2 0 0 1-2-2V5a2 2 0 0 1 2-2h11l5 5v11a2 2 0 0 1-2 2z"
                          ></path>
                          <polyline points="17 21 17 13 7 13 7 21"></polyline>
                          <polyline points="7 3 7 8 15 8"></polyline>
                        </svg>
                        حفظ القائمة
                      </button>
                      <p class="tts-hint">
                        اكتب اسم مستخدم واحد في كل سطر. سيتم قراءة تعليقات هؤلاء
                        المستخدمين فقط.
                      </p>
                    </div>
                  </div>
                </div>

                <div class="tts-column">
                  <!-- مستوى الصوت -->
                  <div class="tts-setting-card">
                    <div class="tts-setting-header">
                      <span class="tts-icon">🔉</span>
                      <span class="tts-setting-title">مستوى الصوت</span>
                      <span class="tts-value" id="ttsVolumeDisplay">100%</span>
                    </div>
                    <div class="tts-slider-container">
                      <input
                        type="range"
                        id="ttsVolume"
                        class="tts-slider"
                        min="0"
                        max="100"
                        value="100"
                        step="1"
                        oninput="updateTTSVolumeDisplay()"
                        onchange="saveTTSSettings()"
                      />
                      <div class="tts-slider-labels">
                        <span>0%</span>
                        <span>50%</span>
                        <span>100%</span>
                      </div>
                    </div>
                  </div>

                  <!-- سرعة القراءة -->
                  <div class="tts-setting-card">
                    <div class="tts-setting-header">
                      <span class="tts-icon">⚡</span>
                      <span class="tts-setting-title">سرعة القراءة</span>
                      <span class="tts-value" id="ttsRateDisplay">1.0x</span>
                    </div>
                    <div class="tts-slider-container">
                      <input
                        type="range"
                        id="ttsRate"
                        class="tts-slider"
                        min="0.5"
                        max="2.0"
                        value="1.0"
                        step="0.1"
                        oninput="updateTTSRateDisplay()"
                        onchange="saveTTSSettings()"
                      />
                      <div class="tts-slider-labels">
                        <span>0.5x</span>
                        <span>1.0x</span>
                        <span>1.5x</span>
                        <span>2.0x</span>
                      </div>
                    </div>
                  </div>

                  <!-- اختبار الصوت -->
                  <div class="tts-setting-card">
                    <button class="tts-test-btn" onclick="testTTSVoice()">
                      <svg
                        xmlns="http://www.w3.org/2000/svg"
                        width="20"
                        height="20"
                        viewBox="0 0 24 24"
                        fill="none"
                        stroke="currentColor"
                        stroke-width="2"
                        stroke-linecap="round"
                        stroke-linejoin="round"
                      >
                        <polygon points="5 3 19 12 5 21 5 3"></polygon>
                      </svg>
                      اختبار الصوت
                    </button>
                  </div>
                </div>
              </div>
            </div>
          </div>

          <!-- صفحة Social Media Rotator (وسائل التواصل الاجتماعي) -->
          <div id="social-rotator-page" class="page">
            <div class="page-header">
              <h2>وسائل التواصل الاجتماعي</h2>
              <p class="page-description">
                عرض روابط حساباتك على مواقع التواصل الاجتماعي بطريقة احترافية
                وجذابة
              </p>
            </div>

            <!-- رابط الويجيت -->
            <div class="card">
              <div class="connection-info">
                <div class="info-item">
                  <label>رابط ويجيت وسائل التواصل:</label>
                  <div class="url-controls-container">
                    <button
                      class="settings-btn"
                      onclick="openSocialRotatorSettings()"
                      title="إعدادات وسائل التواصل"
                    >
                      <i class="fas fa-cog"></i>
                    </button>
                    <div class="code-display">
                      <span id="socialRotatorUrl">جاري التحميل...</span>
                      <button
                        class="copy-btn"
                        onclick="copyToClipboard('socialRotatorUrl')"
                      >
                        نسخ
                      </button>
                    </div>
                  </div>
                </div>
              </div>
            </div>

            <!-- معاينة الويجيت -->
            <div class="card">
              <div class="widget-preview-container">
                <iframe
                  id="socialRotatorPreview"
                  frameborder="0"
                  style="width: 100%; height: 400px; border-radius: 8px"
                >
                </iframe>
              </div>
            </div>
          </div>

          <!-- صفحة Overlay Composer (تجميع الروابط) -->
          <div id="overlay-composer-page" class="page">
            <div class="page-header composer-page-header">
              <h2>
                <svg
                  width="28"
                  height="28"
                  viewBox="0 0 24 24"
                  fill="none"
                  stroke="currentColor"
                  stroke-width="2"
                >
                  <rect x="3" y="3" width="7" height="7"></rect>
                  <rect x="14" y="3" width="7" height="7"></rect>
                  <rect x="14" y="14" width="7" height="7"></rect>
                  <rect x="3" y="14" width="7" height="7"></rect>
                </svg>
                تجميع الروابط
              </h2>
              <p class="page-description">
                قم بدمج جميع الويدجتات في صفحة واحدة لاستخدامها في OBS
              </p>
            </div>

            <!-- رابط العرض النهائي -->
            <div class="card composer-link-card">
              <div class="connection-info">
                <div class="info-item">
                  <label>رابط Overlay المجمّع:</label>
                  <div class="url-controls-container">
                    <div class="code-display">
                      <span id="composerViewUrl">جاري التحميل...</span>
                      <button
                        class="copy-btn"
                        onclick="copyToClipboard('composerViewUrl')"
                      >
                        نسخ
                      </button>
                    </div>
                  </div>
                </div>
              </div>
            </div>

            <!-- منطقة التصميم -->
            <div class="card composer-canvas-card">
              <div class="composer-toolbar">
                <button
                  class="toolbar-btn add-widget-btn"
                  onclick="openWidgetSelector()"
                >
                  <svg width="16" height="16" viewBox="0 0 24 24" fill="none">
                    <line x1="12" y1="5" x2="12" y2="19"></line>
                    <line x1="5" y1="12" x2="19" y2="12"></line>
                  </svg>
                  إضافة Widget
                </button>
                <button
                  class="toolbar-btn save-layout-btn"
                  onclick="saveComposerLayout()"
                >
                  <svg width="16" height="16" viewBox="0 0 24 24" fill="none">
                    <path
                      d="M19 21H5a2 2 0 0 1-2-2V5a2 2 0 0 1 2-2h11l5 5v11a2 2 0 0 1-2 2z"
                    ></path>
                    <polyline points="17 21 17 13 7 13 7 21"></polyline>
                    <polyline points="7 3 7 8 15 8"></polyline>
                  </svg>
                  حفظ التخطيط
                </button>
                <button
                  class="toolbar-btn clear-btn"
                  onclick="clearAllWidgets()"
                >
                  <svg width="16" height="16" viewBox="0 0 24 24" fill="none">
                    <polyline points="3 6 5 6 21 6"></polyline>
                    <path
                      d="M19 6v14a2 2 0 0 1-2 2H7a2 2 0 0 1-2-2V6m3 0V4a2 2 0 0 1 2-2h4a2 2 0 0 1 2 2v2"
                    ></path>
                  </svg>
                  حذف الكل
                </button>
                <div class="canvas-size-selector">
                  <label>
                    <svg
                      width="14"
                      height="14"
                      viewBox="0 0 24 24"
                      fill="none"
                      stroke="currentColor"
                      stroke-width="2"
                    >
                      <rect
                        x="2"
                        y="7"
                        width="20"
                        height="15"
                        rx="2"
                        ry="2"
                      ></rect>
                      <polyline points="17 2 12 7 7 2"></polyline>
                    </svg>
                    حجم الشاشة:
                  </label>
                  <select id="canvasSizePreset" onchange="applyCanvasPreset()">
                    <option value="1920x1080">1920×1080 (أفقي)</option>
                    <option value="1080x1920">1080×1920 (عمودي)</option>
                    <option value="custom">مخصص</option>
                  </select>
                </div>
                <div
                  class="canvas-custom-toolbar"
                  id="canvasCustomToolbar"
                  style="display: none"
                >
                  <div class="dimension-input-toolbar">
                    <label>العرض:</label>
                    <input
                      type="number"
                      id="canvasWidth"
                      value="1920"
                      min="100"
                      max="3840"
                    />
                  </div>
                  <div class="dimension-input-toolbar">
                    <label>الارتفاع:</label>
                    <input
                      type="number"
                      id="canvasHeight"
                      value="1080"
                      min="100"
                      max="2160"
                    />
                  </div>
                  <button
                    class="apply-btn-toolbar"
                    onclick="applyCustomDimensions()"
                  >
                    تطبيق
                  </button>
                </div>
                <div class="toolbar-info">
                  <span id="canvasDimensionsDisplay">1920 × 1080</span>
                </div>
              </div>

              <div class="composer-main-area">
                <!-- قائمة الطبقات -->
                <div class="layers-panel">
                  <div class="layers-header">
                    <h4>
                      <svg
                        width="18"
                        height="18"
                        viewBox="0 0 24 24"
                        fill="none"
                        stroke="currentColor"
                        stroke-width="2"
                      >
                        <polygon points="12 2 2 7 12 12 22 7 12 2"></polygon>
                        <polyline points="2 17 12 22 22 17"></polyline>
                        <polyline points="2 12 12 17 22 12"></polyline>
                      </svg>
                      الطبقات
                    </h4>
                    <span class="layers-count" id="layersCount">0</span>
                  </div>
                  <div class="layers-list" id="layersList">
                    <div class="no-layers">
                      <span>لا توجد طبقات</span>
                      <p>اضغط "➕ إضافة Widget" للبدء</p>
                    </div>
                  </div>
                </div>

                <!-- Canvas -->
                <div class="composer-canvas-wrapper" id="composerCanvasWrapper">
                  <div class="composer-canvas" id="composerCanvas">
                    <!-- سيتم إضافة الويدجتات هنا -->
                  </div>
                </div>
              </div>
            </div>
          </div>
        </main>
      </div>
    </div>

    <div id="messageToast" class="toast"></div>

    <!-- نافذة تأكيد مخصصة -->
    <div id="customConfirmModal" class="custom-confirm-modal">
      <div class="confirm-backdrop"></div>
      <div class="confirm-dialog">
        <div class="confirm-icon">
          <span id="confirmIconText">⚠️</span>
        </div>
        <div class="confirm-content">
          <h3 id="confirmTitle">تأكيد العملية</h3>
          <p id="confirmMessage">هل أنت متأكد من هذا الإجراء؟</p>
        </div>
        <div class="confirm-actions">
          <button id="confirmCancelBtn" class="confirm-btn confirm-cancel">
            <span>إلغاء</span>
          </button>
          <button id="confirmOkBtn" class="confirm-btn confirm-ok">
            <span id="confirmOkText">تأكيد</span>
          </button>
        </div>
      </div>
    </div>

    <!-- نافذة إعدادات Podium -->
    <div id="podiumSettingsModal" class="modal-overlay" style="display: none">
      <div class="settings-modal">
        <div class="settings-header">
          <h3>⚙️ إعدادات عرض المتفاعلين</h3>
          <button class="close-modal-btn" onclick="closePodiumSettings()">
            <i class="fas fa-times"></i>
          </button>
        </div>
        <div class="settings-body">
          <!-- اختيار التصميم -->
          <div class="setting-item">
            <label for="likersThemeSelect">
              <i class="fas fa-palette"></i>
              التصميم
            </label>
            <div class="select-wrapper">
              <select id="likersThemeSelect" onchange="updateLikersTheme()">
                <option value="horizontal-podium">تصميم أفقي كلاسيكي 🏆</option>
                <option value="bear-top3">تصميم الدب - أفضل 3 🐻</option>
                <option value="cyber-blade-podium">
                  Cyber Blade - أفضل 3 ⚔️
                </option>
              </select>
            </div>
            <p class="setting-description">
              اختر التصميم المفضل لعرض أفضل المتفاعلين
            </p>
          </div>

          <!-- عدد المراكز (يتغير حسب التصميم) -->
          <div class="setting-item" id="likersRanksContainer">
            <label for="ranksCountSelect">
              <i class="fas fa-trophy"></i>
              عدد المراكز المعروضة
            </label>
            <div class="select-wrapper">
              <select id="ranksCountSelect" onchange="updateRanksCount()">
                <option value="1">1 مركز</option>
                <option value="2">2 مركز</option>
                <option value="3">3 مراكز</option>
                <option value="4">4 مراكز</option>
                <option value="5">5 مراكز</option>
                <option value="6">6 مراكز</option>
                <option value="7">7 مراكز</option>
                <option value="8">8 مراكز</option>
                <option value="9">9 مراكز</option>
                <option value="10" selected>10 مراكز</option>
              </select>
            </div>
            <p class="setting-description">
              حدد عدد المراكز التي تريد عرضها في ويدجت أفضل المتفاعلين
            </p>
          </div>

          <!-- اتجاه التصميم (Flip) -->
          <div class="setting-item">
            <label for="podiumFlipToggle">
              <i class="fas fa-exchange-alt"></i>
              اتجاه التصميم
            </label>
            <div
              class="toggle-container"
              style="display: flex; align-items: center; gap: 15px"
            >
              <label class="switch">
                <input
                  type="checkbox"
                  id="podiumFlipToggle"
                  onchange="togglePodiumFlip()"
                />
                <span class="slider round"></span>
              </label>
              <span id="podiumFlipStatus" style="color: #888"
                >يمين إلى يسار (RTL)</span
              >
            </div>
            <p class="setting-description">
              عكس اتجاه عرض التصميم (يطبق على جميع تصاميم المتفاعلين)
            </p>
          </div>
        </div>
        <div class="settings-footer">
          <button class="save-settings-btn" onclick="savePodiumSettings()">
            <i class="fas fa-save"></i> حفظ التغييرات
          </button>
        </div>
      </div>
    </div>

    <!-- نافذة إعدادات الداعمين -->
    <div
      id="supportersSettingsModal"
      class="modal-overlay"
      style="display: none"
    >
      <div class="settings-modal">
        <div class="settings-header">
          <h3>⚙️ إعدادات عرض الداعمين</h3>
          <button class="close-modal-btn" onclick="closeSupportersSettings()">
            <i class="fas fa-times"></i>
          </button>
        </div>
        <div class="settings-body">
          <!-- اختيار التصميم -->
          <div class="setting-item">
            <label for="supportersThemeSelect">
              <i class="fas fa-palette"></i>
              التصميم
            </label>
            <div class="select-wrapper">
              <select
                id="supportersThemeSelect"
                onchange="updateSupportersTheme()"
              >
                <option value="horizontal-podium-supporters">
                  تصميم أفقي كلاسيكي 🏆
                </option>
                <option value="bear-top3-supporters">
                  تصميم الدب - أفضل 3 🐻
                </option>
                <option value="cyber-blade-podium-supporters">
                  Cyber Blade - أفضل 3 ⚔️
                </option>
              </select>
            </div>
            <p class="setting-description">
              اختر التصميم المفضل لعرض أفضل الداعمين
            </p>
          </div>

          <!-- عدد المراكز (يتغير حسب التصميم) -->
          <div class="setting-item" id="supportersRanksContainer">
            <label for="supportersRanksCountSelect">
              <i class="fas fa-trophy"></i>
              عدد المراكز المعروضة
            </label>
            <div class="select-wrapper">
              <select
                id="supportersRanksCountSelect"
                onchange="updateSupportersRanksCount()"
              >
                <option value="1">1 مركز</option>
                <option value="2">2 مركز</option>
                <option value="3">3 مراكز</option>
                <option value="4">4 مراكز</option>
                <option value="5">5 مراكز</option>
                <option value="6">6 مراكز</option>
                <option value="7">7 مراكز</option>
                <option value="8">8 مراكز</option>
                <option value="9">9 مراكز</option>
                <option value="10" selected>10 مراكز</option>
              </select>
            </div>
            <p class="setting-description">
              حدد عدد المراكز التي تريد عرضها في ويدجت أفضل الداعمين
            </p>
          </div>

          <div class="setting-item">
            <label for="supportersFlipDirectionSelect">
              <i class="fas fa-arrows-alt-h"></i>
              اتجاه عرض الويدجت
            </label>
            <div class="select-wrapper">
              <select id="supportersFlipDirectionSelect">
                <option value="ltr" selected>من اليسار لليمين (LTR)</option>
                <option value="rtl">من اليمين لليسار (RTL)</option>
              </select>
            </div>
            <p class="setting-description">
              اختر اتجاه عرض الترتيب (الدرج يبدأ من أي جهة)
            </p>
          </div>
        </div>
        <div class="settings-footer">
          <button class="save-settings-btn" onclick="saveSupportersSettings()">
            <i class="fas fa-save"></i> حفظ التغييرات
          </button>
        </div>
      </div>
    </div>

    <!-- نافذة إعدادات الإعجابات -->
    <div id="likesSettingsModal" class="modal-overlay" style="display: none">
      <div class="settings-modal">
        <div class="settings-header">
          <h3>⚙️ إعدادات الإعجابات</h3>
          <button class="close-modal-btn" onclick="closeLikesSettings()">
            <i class="fas fa-times"></i>
          </button>
        </div>
        <div class="settings-body">
          <div class="setting-item">
            <label for="nameColorPicker">
              <i class="fas fa-user"></i>
              لون الاسم
            </label>
            <input type="color" id="nameColorPicker" value="#ff1515" />
            <p class="setting-description">اختر لون اسم المتفاعل</p>
          </div>

          <div class="setting-item">
            <label for="frameColorPicker">
              <i class="fas fa-image"></i>
              لون الإطار
            </label>
            <input type="color" id="frameColorPicker" value="#ff0000" />
            <p class="setting-description">
              اختر لون إطار الصورة وأيقونة الإعجاب
            </p>
          </div>

          <div class="setting-item">
            <label for="likesSoundEnabled">
              <i class="fas fa-volume-up"></i>
              تشغيل صوت
            </label>
            <div class="toggle-switch">
              <input
                type="checkbox"
                id="likesSoundEnabled"
                onchange="toggleLikesSoundOptions()"
              />
              <label for="likesSoundEnabled" class="toggle-label"></label>
            </div>
            <p class="setting-description">تشغيل صوت عند ظهور إعجاب جديد</p>
          </div>

          <div id="likesSoundOptions" style="display: none">
            <div class="setting-item">
              <label>
                <i class="fas fa-music"></i>
                اختيار الصوت
              </label>
              <button
                type="button"
                id="likesSoundLibraryBtn"
                class="sound-source-btn"
                style="width: 100%; margin-top: 8px"
              >
                <i class="fas fa-book"></i>
                مكتبة الأصوات
              </button>

              <div
                id="likesSelectedSoundDisplay"
                class="selected-sound-display"
                style="display: none; margin-top: 12px"
              >
                <div class="selected-sound-info">
                  <span class="sound-icon">🎵</span>
                  <div class="sound-details">
                    <span class="sound-name" id="likesSelectedSoundName"
                      >لم يتم الاختيار</span
                    >
                    <span class="sound-source" id="likesSelectedSoundSource"
                      >المصدر: --</span
                    >
                  </div>
                  <button
                    type="button"
                    class="delete-sound-btn"
                    id="likesDeleteSoundBtn"
                    title="حذف الصوت"
                  >
                    <i class="fas fa-trash"></i>
                  </button>
                </div>
              </div>
            </div>

            <div class="setting-item">
              <label for="likesSoundVolume">
                <i class="fas fa-sliders-h"></i>
                مستوى الصوت
              </label>
              <div class="slider-container">
                <input
                  type="range"
                  id="likesSoundVolume"
                  min="0"
                  max="100"
                  value="80"
                  oninput="document.getElementById('likesSoundVolumeValue').textContent = this.value"
                />
                <span id="likesSoundVolumeValue" class="slider-value">80</span>
              </div>
              <p class="setting-description">اضبط مستوى صوت التأثير (0-100)</p>
            </div>
          </div>
        </div>
        <div class="settings-footer">
          <button class="save-settings-btn" onclick="saveLikesSettings()">
            <i class="fas fa-save"></i> حفظ التغييرات
          </button>
        </div>
      </div>
    </div>

    <!-- نافذة إعدادات الألعاب النارية -->
    <div id="fireworkSettingsModal" class="modal-overlay" style="display: none">
      <div class="settings-modal">
        <div class="settings-header">
          <h3>🎆 إعدادات الألعاب النارية</h3>
          <button class="close-modal-btn" onclick="closeFireworkSettings()">
            <i class="fas fa-times"></i>
          </button>
        </div>
        <div class="settings-body">
          <div class="setting-item">
            <label for="fireworkSoundEnabled">
              <i class="fas fa-volume-up"></i>
              تفعيل الصوت
            </label>
            <div class="toggle-switch">
              <input
                type="checkbox"
                id="fireworkSoundEnabled"
                checked
                onchange="localStorage.setItem('firework_soundenabled', this.checked)"
              />
              <label for="fireworkSoundEnabled" class="toggle-label"></label>
            </div>
            <p class="setting-description">
              تشغيل/إيقاف جميع أصوات الألعاب النارية
            </p>
          </div>

          <div class="setting-item">
            <label for="fireworkSoundVolume">
              <i class="fas fa-sliders-h"></i>
              مستوى الصوت
            </label>
            <div class="slider-container">
              <input
                type="range"
                id="fireworkSoundVolume"
                min="0"
                max="100"
                value="80"
                oninput="document.getElementById('fireworkSoundVolumeValue').textContent = this.value; localStorage.setItem('firework_soundvolume', this.value)"
              />
              <span id="fireworkSoundVolumeValue" class="slider-value">80</span>
            </div>
            <p class="setting-description">اضبط مستوى صوت التأثيرات (0-100)</p>
          </div>

          <div class="setting-item">
            <label for="fireworkMinCoins">
              <i class="fas fa-coins"></i>
              الحد الأدنى للعملات
            </label>
            <input
              type="number"
              id="fireworkMinCoins"
              value="1"
              min="1"
              max="100000"
              class="number-input"
              onchange="localStorage.setItem('firework_mincoins', this.value)"
            />
            <p class="setting-description">الهدايا أقل من هذا الرقم لن تظهر</p>
          </div>

          <div class="setting-item">
            <label for="fireworkMaxConcurrent">
              <i class="fas fa-layer-group"></i>
              الحد الأقصى للألعاب النارية المتزامنة
            </label>
            <input
              type="number"
              id="fireworkMaxConcurrent"
              value="3"
              min="1"
              max="7"
              class="number-input"
              onchange="localStorage.setItem('firework_maxconcurrent', this.value)"
            />
            <p class="setting-description">
              عدد الألعاب النارية التي تظهر في نفس الوقت (افتراضي: 3، أقصى: 7)
            </p>
          </div>
        </div>
        <div class="settings-footer">
          <button class="save-settings-btn" onclick="saveFireworkSettings()">
            <i class="fas fa-save"></i> حفظ التغييرات
          </button>
        </div>
      </div>
    </div>

    <style>
      /* ترتيب خيارات الألوان جنبًا إلى جنب في إعدادات المتابعات */
      .color-row {
        display: flex;
        gap: 12px;
        flex-wrap: wrap;
        margin-bottom: 8px;
      }
      .color-row .setting-item {
        flex: 1;
      }
      .color-row .setting-item .setting-description {
        margin-top: 8px;
      }
      @media (max-width: 720px) {
        .color-row {
          flex-direction: column;
        }
      }
    </style>

    <!-- نافذة إعدادات المتابعات -->
    <div id="followsSettingsModal" class="modal-overlay" style="display: none">
      <div class="settings-modal">
        <div class="settings-header">
          <h3>⚙️ إعدادات المتابعات</h3>
          <button class="close-modal-btn" onclick="closeFollowsSettings()">
            <i class="fas fa-times"></i>
          </button>
        </div>
        <div class="settings-body">
          <div class="color-row">
            <div class="setting-item">
              <label for="followsNameColorPicker">
                <i class="fas fa-user"></i>
                لون الاسم
              </label>
              <input type="color" id="followsNameColorPicker" value="#1900ff" />
              <p class="setting-description">اختر لون اسم المتابع</p>
            </div>

            <div class="setting-item">
              <label for="followsFrameColorPicker">
                <i class="fas fa-image"></i>
                لون الإطار
              </label>
              <input
                type="color"
                id="followsFrameColorPicker"
                value="#3700ff"
              />
              <p class="setting-description">اختر لون إطار الصورة</p>
            </div>

            <div class="setting-item">
              <label for="followsThanksColorPicker">
                <i class="fas fa-heart"></i>
                لون رسالة الشكر
              </label>
              <input
                type="color"
                id="followsThanksColorPicker"
                value="#ffffff"
              />
              <p class="setting-description">
                اختر لون رسالة "شكرا على المتابعه"
              </p>
            </div>
          </div>

          <div class="setting-item">
            <label for="followsSoundEnabled">
              <i class="fas fa-volume-up"></i>
              تشغيل صوت
            </label>
            <div class="toggle-switch">
              <input
                type="checkbox"
                id="followsSoundEnabled"
                onchange="toggleFollowsSoundOptions()"
              />
              <label for="followsSoundEnabled" class="toggle-label"></label>
            </div>
            <p class="setting-description">تشغيل صوت عند ظهور متابع جديد</p>
          </div>

          <div id="followsSoundOptions" style="display: none">
            <div class="setting-item">
              <label>
                <i class="fas fa-music"></i>
                اختيار الصوت
              </label>
              <button
                type="button"
                id="followsSoundLibraryBtn"
                class="sound-source-btn"
                style="width: 100%; margin-top: 8px"
              >
                <i class="fas fa-book"></i>
                مكتبة الأصوات
              </button>

              <div
                id="followsSelectedSoundDisplay"
                class="selected-sound-display"
                style="display: none; margin-top: 12px"
              >
                <div class="selected-sound-info">
                  <span class="sound-icon">🎵</span>
                  <div class="sound-details">
                    <span class="sound-name" id="followsSelectedSoundName"
                      >لم يتم الاختيار</span
                    >
                    <span class="sound-source" id="followsSelectedSoundSource"
                      >المصدر: --</span
                    >
                  </div>
                  <button
                    type="button"
                    class="delete-sound-btn"
                    id="followsDeleteSoundBtn"
                    title="حذف الصوت"
                  >
                    <i class="fas fa-trash"></i>
                  </button>
                </div>
              </div>
            </div>

            <div class="setting-item">
              <label for="followsSoundVolume">
                <i class="fas fa-sliders-h"></i>
                مستوى الصوت
              </label>
              <div class="slider-container">
                <input
                  type="range"
                  id="followsSoundVolume"
                  min="0"
                  max="100"
                  value="80"
                  oninput="document.getElementById('followsSoundVolumeValue').textContent = this.value"
                />
                <span id="followsSoundVolumeValue" class="slider-value"
                  >80</span
                >
              </div>
              <p class="setting-description">اضبط مستوى صوت التأثير (0-100)</p>
            </div>
          </div>
        </div>
        <div class="settings-footer">
          <button class="save-settings-btn" onclick="saveFollowsSettings()">
            <i class="fas fa-save"></i> حفظ التغييرات
          </button>
        </div>
      </div>
    </div>

    <!-- نافذة إعدادات الهدف -->
    <div id="goalSettingsModal" class="modal-overlay" style="display: none">
      <div class="settings-modal">
        <div class="settings-header">
          <h3>⚙️ إعدادات الهدف</h3>
          <button class="close-modal-btn" onclick="closeGoalSettings()">
            <i class="fas fa-times"></i>
          </button>
        </div>
        <div class="settings-body">
          <div class="setting-item">
            <label for="goalValueInput">
              <i class="fas fa-bullseye"></i>
              قيمة الهدف
            </label>
            <div class="goal-value-control">
              <input
                type="number"
                id="goalValueInput"
                value="50"
                min="1"
                max="10000"
                class="goal-value-input"
              />
              <span class="goal-value-label">هدية</span>
            </div>
            <p class="setting-description">
              حدد عدد هدايا Heart Me المطلوبة لإنجاز الهدف
            </p>
          </div>
        </div>
        <div class="settings-footer">
          <button class="save-settings-btn" onclick="saveGoalSettings()">
            <i class="fas fa-save"></i> حفظ التغييرات
          </button>
        </div>
      </div>
    </div>

    <!-- نافذة إعدادات هدف المتابعة -->
    <div
      id="followGoalSettingsModal"
      class="modal-overlay"
      style="display: none"
    >
      <div class="settings-modal">
        <div class="settings-header">
          <h3>⚙️ إعدادات هدف المتابعة</h3>
          <button class="close-modal-btn" onclick="closeFollowGoalSettings()">
            <i class="fas fa-times"></i>
          </button>
        </div>
        <div class="settings-body">
          <div class="setting-item">
            <label for="followGoalValueInput">
              <i class="fas fa-bullseye"></i>
              قيمة الهدف
            </label>
            <div class="goal-value-control">
              <input
                type="number"
                id="followGoalValueInput"
                value="100"
                min="1"
                max="10000"
                class="goal-value-input"
              />
              <span class="goal-value-label">متابع</span>
            </div>
            <p class="setting-description">
              حدد عدد المتابعين المطلوب لإنجاز الهدف
            </p>
          </div>

          <div class="setting-item">
            <label for="followGoalColorPicker">
              <i class="fas fa-palette"></i>
              لون الهدف
            </label>
            <div class="color-control">
              <input
                type="color"
                id="followGoalColorPicker"
                value="#0011ff"
                class="color-picker"
              />
              <span id="followGoalColorValue" class="color-value">#0011ff</span>
            </div>
            <p class="setting-description">
              اختر اللون الرئيسي للهدف (سيتم تطبيقه على جميع التدرجات)
            </p>
          </div>
        </div>
        <div class="settings-footer">
          <button class="save-settings-btn" onclick="saveFollowGoalSettings()">
            <i class="fas fa-save"></i> حفظ التغييرات
          </button>
        </div>
      </div>
    </div>

    <!-- نافذة إعدادات هدف المشاركة (Share Goal) -->
    <div
      id="shareGoalSettingsModal"
      class="modal-overlay"
      style="display: none"
    >
      <div class="settings-modal">
        <div class="settings-header">
          <h3>⚙️ إعدادات هدف المشاركة</h3>
          <button class="close-modal-btn" onclick="closeShareGoalSettings()">
            <i class="fas fa-times"></i>
          </button>
        </div>
        <div class="settings-body">
          <div class="setting-item">
            <label for="shareGoalValueInput">
              <i class="fas fa-bullseye"></i>
              قيمة الهدف
            </label>
            <div class="goal-value-control">
              <input
                type="number"
                id="shareGoalValueInput"
                value="100"
                min="1"
                max="10000"
                class="goal-value-input"
              />
              <span class="goal-value-label">مشاركة</span>
            </div>
            <p class="setting-description">
              حدد عدد المشاركات المطلوب لإنجاز الهدف
            </p>
          </div>

          <div class="setting-item">
            <label for="shareGoalColorPicker">
              <i class="fas fa-palette"></i>
              لون الهدف
            </label>
            <div class="color-control">
              <input
                type="color"
                id="shareGoalColorPicker"
                value="#0011ff"
                class="color-picker"
              />
              <span id="shareGoalColorValue" class="color-value">#0011ff</span>
            </div>
            <p class="setting-description">
              اختر اللون الرئيسي للهدف (سيتم تطبيقه على جميع التدرجات)
            </p>
          </div>
        </div>
        <div class="settings-footer">
          <button class="save-settings-btn" onclick="saveShareGoalSettings()">
            <i class="fas fa-save"></i> حفظ التغييرات
          </button>
        </div>
      </div>
    </div>

    <!-- نافذة إعدادات هدف اللايكات (Like Goal) -->
    <div id="likeGoalSettingsModal" class="modal-overlay" style="display: none">
      <div class="settings-modal">
        <div class="settings-header">
          <h3>⚙️ إعدادات هدف اللايكات</h3>
          <button class="close-modal-btn" onclick="closeLikeGoalSettings()">
            <i class="fas fa-times"></i>
          </button>
        </div>
        <div class="settings-body">
          <div class="setting-item">
            <label for="likeGoalValueInput">
              <i class="fas fa-bullseye"></i>
              قيمة الهدف
            </label>
            <div class="goal-value-control">
              <input
                type="number"
                id="likeGoalValueInput"
                value="100"
                min="1"
                max="10000"
                class="goal-value-input"
              />
              <span class="goal-value-label">لايك</span>
            </div>
            <p class="setting-description">
              حدد عدد اللايكات المطلوب لإنجاز الهدف
            </p>
          </div>

          <div class="setting-item">
            <label for="likeGoalColorPicker">
              <i class="fas fa-palette"></i>
              لون الهدف
            </label>
            <div class="color-control">
              <input
                type="color"
                id="likeGoalColorPicker"
                value="#ff1515"
                class="color-picker"
              />
              <span id="likeGoalColorValue" class="color-value">#ff1515</span>
            </div>
            <p class="setting-description">
              اختر اللون الرئيسي للهدف (سيتم تطبيقه على جميع التدرجات)
            </p>
          </div>
        </div>
        <div class="settings-footer">
          <button class="save-settings-btn" onclick="saveLikeGoalSettings()">
            <i class="fas fa-save"></i> حفظ التغييرات
          </button>
        </div>
      </div>
    </div>

    <!-- نافذة مكتبة الأصوات -->
    <div id="soundLibraryModal" class="modal-overlay" style="display: none">
      <div class="sound-library-modal">
        <div class="sound-library-header">
          <h3>🎵 مكتبة الأصوات</h3>
          <button class="close-modal-btn" id="closeSoundLibraryBtn">
            <i class="fas fa-times"></i>
          </button>
        </div>
        <div class="sound-library-body">
          <div class="sound-search-box">
            <input
              type="text"
              id="soundSearchInput"
              placeholder="ابحث عن صوت..."
            />
            <i class="fas fa-search"></i>
          </div>
          <div id="soundLibraryList" class="sound-library-list">
            <div class="loading-sounds">
              <div class="spinner-circle"></div>
              <p>جاري تحميل الأصوات...</p>
            </div>
          </div>
        </div>
      </div>
    </div>

    <!-- نافذة الإعدادات المتقدمة -->
    <div id="advancedDisplayModal" class="modal-overlay" style="display: none">
      <div class="settings-modal advanced-display-modal">
        <div class="settings-header">
          <h3>⚙️ إعدادات العرض المتقدمة</h3>
          <button class="close-modal-btn" id="closeAdvancedDisplayBtn">
            <i class="fas fa-times"></i>
          </button>
        </div>
        <div class="settings-body advanced-settings-body">
          <!-- حجم الصورة -->
          <div class="setting-item">
            <label for="profileImageSize">
              <i class="fas fa-image"></i>
              حجم صورة البروفايل
            </label>
            <div class="size-control">
              <input
                type="range"
                id="profileImageSize"
                min="50"
                max="200"
                value="150"
                class="size-slider"
              />
              <span id="profileImageSizeValue" class="size-value">150px</span>
            </div>
            <p class="setting-description">
              اضبط حجم صورة البروفايل (50-200 بكسل)
            </p>
          </div>

          <!-- حجم اسم المستخدم -->
          <div class="setting-item">
            <label for="usernameFontSize">
              <i class="fas fa-text-height"></i>
              حجم خط الاسم
            </label>
            <div class="size-control">
              <input
                type="range"
                id="usernameFontSize"
                min="10"
                max="100"
                value="34"
                class="size-slider"
              />
              <span id="usernameFontSizeValue" class="size-value">34px</span>
            </div>
            <p class="setting-description">
              اضبط حجم خط اسم المستخدم (10-100 بكسل)
            </p>
          </div>

          <!-- حجم الرسالة -->
          <div class="setting-item">
            <label for="messageFontSize">
              <i class="fas fa-font"></i>
              حجم خط الرسالة
            </label>
            <div class="size-control">
              <input
                type="range"
                id="messageFontSize"
                min="10"
                max="100"
                value="24"
                class="size-slider"
              />
              <span id="messageFontSizeValue" class="size-value">24px</span>
            </div>
            <p class="setting-description">اضبط حجم خط الرسالة (10-100 بكسل)</p>
          </div>

          <!-- نوع الخط للاسم -->
          <div class="setting-item">
            <label for="usernameFontFamily">
              <i class="fas fa-font"></i>
              خط الاسم
            </label>
            <div class="select-wrapper">
              <select id="usernameFontFamily" class="font-select">
                <option value="system-ui">النظام الافتراضي</option>
                <option value="Arial">Arial</option>
                <option value="'Times New Roman'">Times New Roman</option>
                <option value="'Courier New'">Courier New</option>
                <option value="Georgia">Georgia</option>
                <option value="Verdana">Verdana</option>
                <option value="'Comic Sans MS'">Comic Sans MS</option>
                <option value="'Trebuchet MS'">Trebuchet MS</option>
                <option value="Impact">Impact</option>
                <option value="'Lucida Console'">Lucida Console</option>
                <option value="Tahoma">Tahoma</option>
                <option value="'Palatino Linotype'">Palatino</option>
                <option value="'Arabic Typesetting'">Arabic Typesetting</option>
                <option value="'Simplified Arabic'">Simplified Arabic</option>
                <option value="'Traditional Arabic'">Traditional Arabic</option>
                <option value="'Segoe UI'">Segoe UI</option>
              </select>
            </div>
            <p class="setting-description">اختر نوع الخط لاسم المستخدم</p>
          </div>

          <!-- نوع الخط للرسالة -->
          <div class="setting-item">
            <label for="messageFontFamily">
              <i class="fas fa-font"></i>
              خط الرسالة
            </label>
            <div class="select-wrapper">
              <select id="messageFontFamily" class="font-select">
                <option value="system-ui">النظام الافتراضي</option>
                <option value="Arial">Arial</option>
                <option value="'Times New Roman'">Times New Roman</option>
                <option value="'Courier New'">Courier New</option>
                <option value="Georgia">Georgia</option>
                <option value="Verdana">Verdana</option>
                <option value="'Comic Sans MS'">Comic Sans MS</option>
                <option value="'Trebuchet MS'">Trebuchet MS</option>
                <option value="Impact">Impact</option>
                <option value="'Lucida Console'">Lucida Console</option>
                <option value="Tahoma">Tahoma</option>
                <option value="'Palatino Linotype'">Palatino</option>
                <option value="'Arabic Typesetting'">Arabic Typesetting</option>
                <option value="'Simplified Arabic'">Simplified Arabic</option>
                <option value="'Traditional Arabic'">Traditional Arabic</option>
                <option value="'Segoe UI'">Segoe UI</option>
              </select>
            </div>
            <p class="setting-description">اختر نوع الخط للرسالة</p>
          </div>

          <!-- لون الاسم -->
          <div class="setting-item">
            <label for="usernameColor">
              <i class="fas fa-palette"></i>
              لون الاسم
            </label>
            <div class="color-control">
              <input
                type="color"
                id="usernameColor"
                value="#ffffff"
                class="color-picker"
              />
              <span id="usernameColorValue" class="color-value">#ffffff</span>
            </div>
            <p class="setting-description">اختر لون اسم المستخدم</p>
          </div>

          <!-- لون الرسالة -->
          <div class="setting-item">
            <label for="messageColor">
              <i class="fas fa-palette"></i>
              لون الرسالة
            </label>
            <div class="color-control">
              <input
                type="color"
                id="messageColor"
                value="#ffffff"
                class="color-picker"
              />
              <span id="messageColorValue" class="color-value">#ffffff</span>
            </div>
            <p class="setting-description">اختر لون الرسالة</p>
          </div>

          <!-- ظل النص للاسم -->
          <div class="setting-item">
            <label class="checkbox-label">
              <input type="checkbox" id="usernameTextShadow" checked />
              <span class="checkmark"></span>
              <span class="label-text">
                <i class="fas fa-shadow"></i>
                تمكين ظل النص للاسم
              </span>
            </label>
            <p class="setting-description">يضيف ظل للنص لتحسين الوضوح</p>
          </div>

          <!-- ظل النص للرسالة -->
          <div class="setting-item">
            <label class="checkbox-label">
              <input type="checkbox" id="messageTextShadow" checked />
              <span class="checkmark"></span>
              <span class="label-text">
                <i class="fas fa-shadow"></i>
                تمكين ظل النص للرسالة
              </span>
            </label>
            <p class="setting-description">يضيف ظل للنص لتحسين الوضوح</p>
          </div>
        </div>
        <div class="settings-footer">
          <button class="reset-defaults-btn" id="resetAdvancedDefaults">
            <i class="fas fa-undo"></i> استعادة الافتراضي
          </button>
          <button class="save-settings-btn" id="saveAdvancedDisplay">
            <i class="fas fa-check"></i> تطبيق
          </button>
        </div>
      </div>
    </div>

    <!-- 💳 Modal الاشتراك المنبثق -->
    <div
      id="subscriptionModal"
      class="subscription-modal"
      style="display: none"
    >
      <div class="subscription-modal-content">
        <div class="subscription-modal-header">
          <button class="close-subscription-modal">&times;</button>
        </div>
        <div class="subscription-modal-body">
          <!-- صندوق الاشتراك الحديث الكامل -->
          <div class="modern-subscription-box modal-subscription-box">
            <!-- العنوان الرئيسي -->
            <div class="subscription-box-header">
              <div class="header-icon">
                <svg
                  xmlns="http://www.w3.org/2000/svg"
                  width="28"
                  height="28"
                  viewBox="0 0 24 24"
                  fill="none"
                  stroke="currentColor"
                  stroke-width="2"
                  stroke-linecap="round"
                  stroke-linejoin="round"
                >
                  <path d="M12 2L2 7l10 5 10-5-10-5z"></path>
                  <path d="M2 17l10 5 10-5"></path>
                  <path d="M2 12l10 5 10-5"></path>
                </svg>
              </div>
              <h3>يجب عليك الاشتراك لاستخدام جميع الميزات</h3>
            </div>

            <!-- جدول الميزات -->
            <div class="features-table">
              <div class="feature-item">
                <svg
                  class="check-icon"
                  xmlns="http://www.w3.org/2000/svg"
                  width="20"
                  height="20"
                  viewBox="0 0 24 24"
                  fill="none"
                  stroke="currentColor"
                  stroke-width="3"
                  stroke-linecap="round"
                  stroke-linejoin="round"
                >
                  <polyline points="20 6 9 17 4 12"></polyline>
                </svg>
                <span>إشعارات فورية للمتابعات والإعجابات</span>
              </div>
              <div class="feature-item">
                <svg
                  class="check-icon"
                  xmlns="http://www.w3.org/2000/svg"
                  width="20"
                  height="20"
                  viewBox="0 0 24 24"
                  fill="none"
                  stroke="currentColor"
                  stroke-width="3"
                  stroke-linecap="round"
                  stroke-linejoin="round"
                >
                  <polyline points="20 6 9 17 4 12"></polyline>
                </svg>
                <span>عرض التعليقات على الشاشة مباشرة</span>
              </div>
              <div class="feature-item">
                <svg
                  class="check-icon"
                  xmlns="http://www.w3.org/2000/svg"
                  width="20"
                  height="20"
                  viewBox="0 0 24 24"
                  fill="none"
                  stroke="currentColor"
                  stroke-width="3"
                  stroke-linecap="round"
                  stroke-linejoin="round"
                >
                  <polyline points="20 6 9 17 4 12"></polyline>
                </svg>
                <span>إجراءات تلقائية للهدايا والأحداث</span>
              </div>
              <div class="feature-item">
                <svg
                  class="check-icon"
                  xmlns="http://www.w3.org/2000/svg"
                  width="20"
                  height="20"
                  viewBox="0 0 24 24"
                  fill="none"
                  stroke="currentColor"
                  stroke-width="3"
                  stroke-linecap="round"
                  stroke-linejoin="round"
                >
                  <polyline points="20 6 9 17 4 12"></polyline>
                </svg>
                <span>لوحة تحكم متقدمة للأهداف</span>
              </div>
              <div class="feature-item">
                <svg
                  class="check-icon"
                  xmlns="http://www.w3.org/2000/svg"
                  width="20"
                  height="20"
                  viewBox="0 0 24 24"
                  fill="none"
                  stroke="currentColor"
                  stroke-width="3"
                  stroke-linecap="round"
                  stroke-linejoin="round"
                >
                  <polyline points="20 6 9 17 4 12"></polyline>
                </svg>
                <span>ويدجت قائمة الداعمين والمتصدرين</span>
              </div>
              <div class="feature-item">
                <svg
                  class="check-icon"
                  xmlns="http://www.w3.org/2000/svg"
                  width="20"
                  height="20"
                  viewBox="0 0 24 24"
                  fill="none"
                  stroke="currentColor"
                  stroke-width="3"
                  stroke-linecap="round"
                  stroke-linejoin="round"
                >
                  <polyline points="20 6 9 17 4 12"></polyline>
                </svg>
                <span>تحديثات مستمرة ودعم فني متواصل</span>
              </div>
            </div>

            <!-- قسم السعر والاشتراك -->
            <div class="subscription-footer">
              <!-- زر الاشتراك مباشرة بدون حقول خصم -->
              <div class="subscribe-action">
                <button
                  class="subscribe-btn-new modal-btn-modern"
                  data-plan="monthly"
                >
                  <svg
                    class="btn-icon"
                    xmlns="http://www.w3.org/2000/svg"
                    width="20"
                    height="20"
                    viewBox="0 0 24 24"
                    fill="none"
                    stroke="currentColor"
                    stroke-width="2"
                    stroke-linecap="round"
                    stroke-linejoin="round"
                  >
                    <circle cx="12" cy="12" r="10"></circle>
                    <polyline points="12 6 12 12 16 14"></polyline>
                  </svg>
                  <span>اشترك الآن</span>
                </button>
                <div class="price-display">
                  <span class="price-amount" id="modalDisplayedPrice"
                    >$9.99</span
                  >
                  <span class="price-period">/شهر</span>
                </div>
              </div>

              <!-- ملاحظة الأمان -->
              <div class="security-note">
                <svg
                  xmlns="http://www.w3.org/2000/svg"
                  width="14"
                  height="14"
                  viewBox="0 0 24 24"
                  fill="none"
                  stroke="currentColor"
                  stroke-width="2"
                  stroke-linecap="round"
                  stroke-linejoin="round"
                >
                  <rect
                    x="3"
                    y="11"
                    width="18"
                    height="11"
                    rx="2"
                    ry="2"
                  ></rect>
                  <path d="M7 11V7a5 5 0 0 1 10 0v4"></path>
                </svg>
                <span>دفع آمن • إلغاء مجاني في أي وقت</span>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>

    <!-- نافذة اختيار Widget -->
    <div id="widgetSelectorModal" class="modal-overlay" style="display: none">
      <div class="modal-dialog widget-selector-modal">
        <div class="modal-header">
          <h3>
            <svg
              width="24"
              height="24"
              viewBox="0 0 24 24"
              fill="none"
              stroke="currentColor"
              stroke-width="2"
            >
              <rect x="3" y="3" width="7" height="7"></rect>
              <rect x="14" y="3" width="7" height="7"></rect>
              <rect x="14" y="14" width="7" height="7"></rect>
              <rect x="3" y="14" width="7" height="7"></rect>
            </svg>
            اختر Widget للإضافة
          </h3>
          <button class="modal-close" onclick="closeWidgetSelector()">
            &times;
          </button>
        </div>
        <div class="modal-body">
          <!-- قسم الإشعارات والتنبيهات -->
          <div class="widget-category">
            <h4 class="category-title">
              <svg
                width="20"
                height="20"
                viewBox="0 0 24 24"
                fill="none"
                stroke="currentColor"
                stroke-width="2"
              >
                <path d="M18 8A6 6 0 0 0 6 8c0 7-3 9-3 9h18s-3-2-3-9"></path>
                <path d="M13.73 21a2 2 0 0 1-3.46 0"></path>
              </svg>
              الإشعارات والتنبيهات
            </h4>
            <div class="widget-grid">
              <div class="widget-card" onclick="addWidgetToCanvas('comments')">
                <div class="widget-card-icon gradient-blue">
                  <svg
                    width="28"
                    height="28"
                    viewBox="0 0 24 24"
                    fill="none"
                    stroke="currentColor"
                    stroke-width="2"
                  >
                    <path
                      d="M21 15a2 2 0 0 1-2 2H7l-4 4V5a2 2 0 0 1 2-2h14a2 2 0 0 1 2 2z"
                    ></path>
                  </svg>
                </div>
                <h5>التعليقات</h5>
                <p>عرض التعليقات المباشرة</p>
              </div>
              <div class="widget-card" onclick="addWidgetToCanvas('likes')">
                <div class="widget-card-icon gradient-pink">
                  <svg
                    width="28"
                    height="28"
                    viewBox="0 0 24 24"
                    fill="none"
                    stroke="currentColor"
                    stroke-width="2"
                  >
                    <path
                      d="M20.84 4.61a5.5 5.5 0 0 0-7.78 0L12 5.67l-1.06-1.06a5.5 5.5 0 0 0-7.78 7.78l1.06 1.06L12 21.23l7.78-7.78 1.06-1.06a5.5 5.5 0 0 0 0-7.78z"
                    ></path>
                  </svg>
                </div>
                <h5>الإعجابات</h5>
                <p>عرض اللايكات المباشرة</p>
              </div>
              <div class="widget-card" onclick="addWidgetToCanvas('follows')">
                <div class="widget-card-icon gradient-purple">
                  <svg
                    width="28"
                    height="28"
                    viewBox="0 0 24 24"
                    fill="none"
                    stroke="currentColor"
                    stroke-width="2"
                  >
                    <path d="M17 21v-2a4 4 0 0 0-4-4H5a4 4 0 0 0-4 4v2"></path>
                    <circle cx="9" cy="7" r="4"></circle>
                    <path d="M23 21v-2a4 4 0 0 0-3-3.87"></path>
                    <path d="M16 3.13a4 4 0 0 1 0 7.75"></path>
                  </svg>
                </div>
                <h5>المتابعات</h5>
                <p>عرض المتابعين الجدد</p>
              </div>
              <div
                class="widget-card"
                onclick="addWidgetToCanvas('like-hearts')"
              >
                <div class="widget-card-icon gradient-red">
                  <svg
                    width="28"
                    height="28"
                    viewBox="0 0 24 24"
                    fill="none"
                    stroke="currentColor"
                    stroke-width="2"
                  >
                    <path
                      d="M20.84 4.61a5.5 5.5 0 0 0-7.78 0L12 5.67l-1.06-1.06a5.5 5.5 0 0 0-7.78 7.78l1.06 1.06L12 21.23l7.78-7.78 1.06-1.06a5.5 5.5 0 0 0 0-7.78z"
                    ></path>
                    <circle cx="12" cy="12" r="3"></circle>
                  </svg>
                </div>
                <h5>قلوب اللايكات</h5>
                <p>قلوب مع صور المتفاعلين</p>
              </div>
            </div>
          </div>

          <!-- قسم الترتيب والمنافسة -->
          <div class="widget-category">
            <h4 class="category-title">
              <svg
                width="20"
                height="20"
                viewBox="0 0 24 24"
                fill="none"
                stroke="currentColor"
                stroke-width="2"
              >
                <path d="M6 9H4.5a2.5 2.5 0 0 1 0-5H6"></path>
                <path d="M18 9h1.5a2.5 2.5 0 0 0 0-5H18"></path>
                <path d="M4 22h16"></path>
                <path
                  d="M10 14.66V17c0 .55-.47.98-.97 1.21C7.85 18.75 7 20.24 7 22"
                ></path>
                <path
                  d="M14 14.66V17c0 .55.47.98.97 1.21C16.15 18.75 17 20.24 17 22"
                ></path>
                <path d="M18 2H6v7a6 6 0 0 0 12 0V2z"></path>
              </svg>
              الترتيب والمنافسة
            </h4>
            <div class="widget-grid">
              <div class="widget-card" onclick="addWidgetToCanvas('podium')">
                <div class="widget-card-icon gradient-gold">
                  <svg
                    width="28"
                    height="28"
                    viewBox="0 0 24 24"
                    fill="none"
                    stroke="currentColor"
                    stroke-width="2"
                  >
                    <path d="M6 9H4.5a2.5 2.5 0 0 1 0-5H6"></path>
                    <path d="M18 9h1.5a2.5 2.5 0 0 0 0-5H18"></path>
                    <path d="M4 22h16"></path>
                    <path
                      d="M10 14.66V17c0 .55-.47.98-.97 1.21C7.85 18.75 7 20.24 7 22"
                    ></path>
                    <path
                      d="M14 14.66V17c0 .55.47.98.97 1.21C16.15 18.75 17 20.24 17 22"
                    ></path>
                    <path d="M18 2H6v7a6 6 0 0 0 12 0V2z"></path>
                  </svg>
                </div>
                <h5>أفضل المتفاعلين</h5>
                <p>منصة أعلى المشاركين</p>
              </div>
              <div
                class="widget-card"
                onclick="addWidgetToCanvas('podium-supporters')"
              >
                <div class="widget-card-icon gradient-orange">
                  <svg
                    width="28"
                    height="28"
                    viewBox="0 0 24 24"
                    fill="none"
                    stroke="currentColor"
                    stroke-width="2"
                  >
                    <circle cx="12" cy="8" r="7"></circle>
                    <polyline
                      points="8.21 13.89 7 23 12 20 17 23 15.79 13.88"
                    ></polyline>
                  </svg>
                </div>
                <h5>أفضل الداعمين</h5>
                <p>منصة أكبر المساهمين</p>
              </div>
            </div>
          </div>

          <!-- قسم الأهداف -->
          <div class="widget-category">
            <h4 class="category-title">
              <svg
                width="20"
                height="20"
                viewBox="0 0 24 24"
                fill="none"
                stroke="currentColor"
                stroke-width="2"
              >
                <circle cx="12" cy="12" r="10"></circle>
                <circle cx="12" cy="12" r="6"></circle>
                <circle cx="12" cy="12" r="2"></circle>
              </svg>
              الأهداف والتحديات
            </h4>
            <div class="widget-grid">
              <div
                class="widget-card"
                onclick="addWidgetToCanvas('heart-me-goal')"
              >
                <div class="widget-card-icon gradient-pink">
                  <svg
                    width="28"
                    height="28"
                    viewBox="0 0 24 24"
                    fill="none"
                    stroke="currentColor"
                    stroke-width="2"
                  >
                    <path d="M22 11.08V12a10 10 0 1 1-5.93-9.14"></path>
                    <polyline points="22 4 12 14.01 9 11.01"></polyline>
                  </svg>
                </div>
                <h5>هدف Heart Me</h5>
                <p>هدف الهدايا</p>
              </div>
              <div class="widget-card" onclick="addWidgetToCanvas('like-goal')">
                <div class="widget-card-icon gradient-red">
                  <svg
                    width="28"
                    height="28"
                    viewBox="0 0 24 24"
                    fill="none"
                    stroke="currentColor"
                    stroke-width="2"
                  >
                    <path d="M22 11.08V12a10 10 0 1 1-5.93-9.14"></path>
                    <path d="M20.84 4.61a5.5 5.5 0 0 0-7.78 0L12 5.67"></path>
                  </svg>
                </div>
                <h5>هدف اللايكات</h5>
                <p>هدف عدد الإعجابات</p>
              </div>
              <div
                class="widget-card"
                onclick="addWidgetToCanvas('follow-goal')"
              >
                <div class="widget-card-icon gradient-purple">
                  <svg
                    width="28"
                    height="28"
                    viewBox="0 0 24 24"
                    fill="none"
                    stroke="currentColor"
                    stroke-width="2"
                  >
                    <path d="M22 11.08V12a10 10 0 1 1-5.93-9.14"></path>
                    <path d="M16 21v-2a4 4 0 0 0-4-4H5"></path>
                  </svg>
                </div>
                <h5>هدف المتابعة</h5>
                <p>هدف عدد المتابعين</p>
              </div>
              <div
                class="widget-card"
                onclick="addWidgetToCanvas('share-goal')"
              >
                <div class="widget-card-icon gradient-blue">
                  <svg
                    width="28"
                    height="28"
                    viewBox="0 0 24 24"
                    fill="none"
                    stroke="currentColor"
                    stroke-width="2"
                  >
                    <circle cx="18" cy="5" r="3"></circle>
                    <circle cx="6" cy="12" r="3"></circle>
                    <circle cx="18" cy="19" r="3"></circle>
                    <line x1="8.59" y1="13.51" x2="15.42" y2="17.49"></line>
                    <line x1="15.41" y1="6.51" x2="8.59" y2="10.49"></line>
                  </svg>
                </div>
                <h5>هدف المشاركة</h5>
                <p>هدف عدد الشيرات</p>
              </div>
            </div>
          </div>

          <!-- قسم الأدوات والمؤثرات -->
          <div class="widget-category">
            <h4 class="category-title">
              <svg
                width="20"
                height="20"
                viewBox="0 0 24 24"
                fill="none"
                stroke="currentColor"
                stroke-width="2"
              >
                <polygon
                  points="12 2 15.09 8.26 22 9.27 17 14.14 18.18 21.02 12 17.77 5.82 21.02 7 14.14 2 9.27 8.91 8.26 12 2"
                ></polygon>
              </svg>
              الأدوات والمؤثرات
            </h4>
            <div class="widget-grid">
              <div class="widget-card" onclick="addWidgetToCanvas('tts')">
                <div class="widget-card-icon gradient-green">
                  <svg
                    width="28"
                    height="28"
                    viewBox="0 0 24 24"
                    fill="none"
                    stroke="currentColor"
                    stroke-width="2"
                  >
                    <polygon
                      points="11 5 6 9 2 9 2 15 6 15 11 19 11 5"
                    ></polygon>
                    <path
                      d="M19.07 4.93a10 10 0 0 1 0 14.14M15.54 8.46a5 5 0 0 1 0 7.07"
                    ></path>
                  </svg>
                </div>
                <h5>قارئ التعليقات</h5>
                <p>قراءة صوتية للرسائل</p>
              </div>
              <div class="widget-card" onclick="addWidgetToCanvas('firework')">
                <div class="widget-card-icon gradient-rainbow">
                  <svg
                    width="28"
                    height="28"
                    viewBox="0 0 24 24"
                    fill="none"
                    stroke="currentColor"
                    stroke-width="2"
                  >
                    <path
                      d="M12 2v4M12 18v4M4.93 4.93l2.83 2.83M16.24 16.24l2.83 2.83M2 12h4M18 12h4M4.93 19.07l2.83-2.83M16.24 7.76l2.83-2.83"
                    ></path>
                    <circle cx="12" cy="12" r="3"></circle>
                  </svg>
                </div>
                <h5>الألعاب النارية</h5>
                <p>تأثيرات احتفالية</p>
              </div>
            </div>
          </div>

          <!-- قسم الشاشات -->
          <div class="widget-category">
            <h4 class="category-title">
              <svg
                width="20"
                height="20"
                viewBox="0 0 24 24"
                fill="none"
                stroke="currentColor"
                stroke-width="2"
              >
                <rect x="2" y="3" width="20" height="14" rx="2" ry="2"></rect>
                <line x1="8" y1="21" x2="16" y2="21"></line>
                <line x1="12" y1="17" x2="12" y2="21"></line>
              </svg>
              شاشات العرض
            </h4>
            <div class="widget-grid">
              <div class="widget-card" onclick="addWidgetToCanvas('screen1')">
                <div class="widget-card-icon gradient-indigo">
                  <svg
                    width="28"
                    height="28"
                    viewBox="0 0 24 24"
                    fill="none"
                    stroke="currentColor"
                    stroke-width="2"
                  >
                    <rect
                      x="2"
                      y="7"
                      width="20"
                      height="15"
                      rx="2"
                      ry="2"
                    ></rect>
                    <polyline points="17 2 12 7 7 2"></polyline>
                  </svg>
                </div>
                <h5>Screen 1</h5>
                <p>شاشة العرض الأولى</p>
              </div>
              <div class="widget-card" onclick="addWidgetToCanvas('screen2')">
                <div class="widget-card-icon gradient-indigo">
                  <svg
                    width="28"
                    height="28"
                    viewBox="0 0 24 24"
                    fill="none"
                    stroke="currentColor"
                    stroke-width="2"
                  >
                    <rect
                      x="2"
                      y="7"
                      width="20"
                      height="15"
                      rx="2"
                      ry="2"
                    ></rect>
                    <polyline points="17 2 12 7 7 2"></polyline>
                  </svg>
                </div>
                <h5>Screen 2</h5>
                <p>شاشة العرض الثانية</p>
              </div>
              <div class="widget-card" onclick="addWidgetToCanvas('screen3')">
                <div class="widget-card-icon gradient-indigo">
                  <svg
                    width="28"
                    height="28"
                    viewBox="0 0 24 24"
                    fill="none"
                    stroke="currentColor"
                    stroke-width="2"
                  >
                    <rect
                      x="2"
                      y="7"
                      width="20"
                      height="15"
                      rx="2"
                      ry="2"
                    ></rect>
                    <polyline points="17 2 12 7 7 2"></polyline>
                  </svg>
                </div>
                <h5>Screen 3</h5>
                <p>شاشة العرض الثالثة</p>
              </div>
              <div class="widget-card" onclick="addWidgetToCanvas('screen4')">
                <div class="widget-card-icon gradient-indigo">
                  <svg
                    width="28"
                    height="28"
                    viewBox="0 0 24 24"
                    fill="none"
                    stroke="currentColor"
                    stroke-width="2"
                  >
                    <rect
                      x="2"
                      y="7"
                      width="20"
                      height="15"
                      rx="2"
                      ry="2"
                    ></rect>
                    <polyline points="17 2 12 7 7 2"></polyline>
                  </svg>
                </div>
                <h5>Screen 4</h5>
                <p>شاشة العرض الرابعة</p>
              </div>
              <div class="widget-card" onclick="addWidgetToCanvas('screen5')">
                <div class="widget-card-icon gradient-indigo">
                  <svg
                    width="28"
                    height="28"
                    viewBox="0 0 24 24"
                    fill="none"
                    stroke="currentColor"
                    stroke-width="2"
                  >
                    <rect
                      x="2"
                      y="7"
                      width="20"
                      height="15"
                      rx="2"
                      ry="2"
                    ></rect>
                    <polyline points="17 2 12 7 7 2"></polyline>
                  </svg>
                </div>
                <h5>Screen 5</h5>
                <p>شاشة العرض الخامسة</p>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>

    <!-- نافذة تعديل Widget -->
    <div id="widgetPropertiesModal" class="modal-overlay" style="display: none">
      <div class="modal-dialog widget-properties-modal">
        <div class="modal-header">
          <h3>⚙️ خصائص Widget</h3>
          <button class="modal-close" onclick="closeWidgetProperties()">
            &times;
          </button>
        </div>
        <div class="modal-body">
          <div class="property-group">
            <label>الموضع X:</label>
            <input
              type="number"
              id="widgetPropX"
              onchange="updateWidgetProperty('x')"
            />
          </div>
          <div class="property-group">
            <label>الموضع Y:</label>
            <input
              type="number"
              id="widgetPropY"
              onchange="updateWidgetProperty('y')"
            />
          </div>
          <div class="property-group">
            <label>العرض:</label>
            <input
              type="number"
              id="widgetPropWidth"
              onchange="updateWidgetProperty('width')"
            />
          </div>
          <div class="property-group">
            <label>الارتفاع:</label>
            <input
              type="number"
              id="widgetPropHeight"
              onchange="updateWidgetProperty('height')"
            />
          </div>
          <div class="property-group">
            <label>الطبقة (Z-Index):</label>
            <input
              type="number"
              id="widgetPropZIndex"
              min="1"
              onchange="updateWidgetProperty('zIndex')"
            />
          </div>
          <div class="property-actions">
            <button
              class="property-btn save-btn"
              onclick="saveWidgetProperties()"
            >
              ✔️ حفظ التغييرات
            </button>
            <button class="property-btn lock-btn" onclick="toggleWidgetLock()">
              <span id="lockBtnText">🔓 إلغاء القفل</span>
            </button>
            <button
              class="property-btn visibility-btn"
              onclick="toggleWidgetVisibility()"
            >
              <span id="visibilityBtnText">👁️ إخفاء</span>
            </button>
            <button
              class="property-btn delete-btn"
              onclick="deleteSelectedWidget()"
            >
              🗑️ حذف
            </button>
          </div>
        </div>
      </div>
    </div>

    <!-- Social Media Rotator Settings Modal -->
    <div
      id="socialRotatorSettingsModal"
      class="modal-overlay"
      style="display: none"
    >
      <div class="settings-modal">
        <div class="settings-header">
          <h3>⚙ إعدادات وسائل التواصل الاجتماعي</h3>
          <button
            class="close-modal-btn"
            onclick="closeSocialRotatorSettings()"
          >
            &times;
          </button>
        </div>
        <div class="settings-body">
          <div id="platformList" class="platforms-container"></div>
        </div>
        <div class="settings-footer">
          <button class="btn btn-primary" onclick="saveSocialRotatorSettings()">
            <i class="fas fa-save"></i> حفظ الإعدادات
          </button>
        </div>
      </div>
    </div>

    <!-- 💾 نظام إدارة الإعدادات مع localStorage -->
    <script src="/shared/js/settings-cache.js"></script>
    <script src="/dashboard/dashboard.js"></script>
    <script src="/overlay/overlay-composer.js"></script>
  </body>
</html>
<script src="https://cdn.jsdelivr.net/npm/tus-js-client@2.3.0/dist/tus.min.js"></script>
