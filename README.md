<!doctype html>
<html lang="ar" dir="rtl" class="h-full w-full">
 <head>
  <meta charset="UTF-8">
  <title>شاشة عرض مخزون العسل</title><!-- Tailwind CDN -->
  <script src="https://cdn.tailwindcss.com"></script><!-- Canva SDKs -->
  <script src="/_sdk/element_sdk.js"></script><!-- Firebase SDK for Real-time Sync -->
  <script src="https://www.gstatic.com/firebasejs/10.7.1/firebase-app-compat.js"></script>
  <script src="https://www.gstatic.com/firebasejs/10.7.1/firebase-database-compat.js"></script>
  <style>
    body {
      box-sizing: border-box;
    }
    html, body {
      height: 100%;
      width: 100%;
      margin: 0;
      padding: 0;
    }
    .fade-in {
      animation: fadeIn 1.5s ease-out;
    }
    @keyframes fadeIn {
      from { opacity: 0; transform: translateY(12px); }
      to { opacity: 1; transform: translateY(0); }
    }
    .honeycomb-bg {
      background-color: #edebe0;
      background-image:
        radial-gradient(circle at 0 0, rgba(255, 225, 160, 0.08) 0, transparent 55%),
        radial-gradient(circle at 100% 20%, rgba(255, 210, 120, 0.12) 0, transparent 60%),
        radial-gradient(circle at 0 100%, rgba(222, 179, 92, 0.15) 0, transparent 55%);
      background-size: 100% 100%;
    }
    .honeycomb-mask {
      background-image:
        linear-gradient(90deg, rgba(255, 210, 120, 0.25) 1px, transparent 1px),
        linear-gradient(150deg, rgba(255, 230, 170, 0.12) 1px, transparent 1px),
        linear-gradient(210deg, rgba(255, 230, 170, 0.12) 1px, transparent 1px);
      background-size: 24px 42px;
      mask-image: radial-gradient(circle at center, black 0, transparent 70%);
      opacity: 0.35;
    }
    .jar-pulse {
      animation: jarPulse 4s ease-in-out infinite;
    }
    @keyframes jarPulse {
      0%, 100% { transform: translateY(0) scale(1); }
      50% { transform: translateY(-6px) scale(1.03); }
    }
    .smooth-number {
      transition: all 0.6s ease;
    }
    .pulse-ring {
      animation: pulseRing 2s cubic-bezier(0.4, 0, 0.6, 1) infinite;
    }
    @keyframes pulseRing {
      0%, 100% { opacity: 1; transform: scale(1); }
      50% { opacity: 0.5; transform: scale(1.1); }
    }
    .slide-up {
      animation: slideUp 0.8s ease-out;
    }
    @keyframes slideUp {
      from { opacity: 0; transform: translateY(20px); }
      to { opacity: 1; transform: translateY(0); }
    }
    .comment-slide-in {
      animation: commentSlideIn 0.5s ease-out;
    }
    @keyframes commentSlideIn {
      from { opacity: 0; transform: translateX(100px); }
      to { opacity: 1; transform: translateX(0); }
    }
    .comment-slide-out {
      animation: commentSlideOut 0.5s ease-in forwards;
    }
    @keyframes commentSlideOut {
      from { opacity: 1; transform: translateX(0); }
      to { opacity: 0; transform: translateX(100px); }
    }
    .heart-float {
      animation: heartFloat 4s ease-out forwards;
      pointer-events: none;
    }
    @keyframes heartFloat {
      0% {
        opacity: 0;
        transform: translateY(0) translateX(0) scale(0.5) rotate(0deg);
      }
      10% {
        opacity: 1;
        transform: translateY(-40px) translateX(10px) scale(1.1) rotate(15deg);
      }
      30% {
        opacity: 1;
        transform: translateY(-180px) translateX(var(--drift-x)) scale(1) rotate(-10deg);
      }
      60% {
        opacity: 0.9;
        transform: translateY(-380px) translateX(calc(var(--drift-x) * 1.3)) scale(0.85) rotate(20deg);
      }
      100% {
        opacity: 0;
        transform: translateY(-600px) translateX(calc(var(--drift-x) * 1.8)) scale(0.5) rotate(-15deg);
      }
    }
    @keyframes scroll-news {
      0% { transform: translateX(-50%); }
      100% { transform: translateX(0); }
    }
    .center-alert-show {
      animation: centerAlertShow 0.6s cubic-bezier(0.34, 1.56, 0.64, 1);
    }
    @keyframes centerAlertShow {
      0% { opacity: 0; transform: translate(-50%, -50%) scale(0.5); }
      100% { opacity: 1; transform: translate(-50%, -50%) scale(1); }
    }
    .center-alert-hide {
      animation: centerAlertHide 0.4s ease-out forwards;
    }
    @keyframes centerAlertHide {
      0% { opacity: 1; transform: translate(-50%, -50%) scale(1); }
      100% { opacity: 0; transform: translate(-50%, -50%) scale(0.8); }
    }
    .alert-glow-red {
      animation: alertGlowRed 0.8s ease-in-out infinite;
    }
    @keyframes alertGlowRed {
      0%, 100% { 
        border-color: #6b1f2a;
        box-shadow: 0 0 5px rgba(107, 31, 42, 0.3);
      }
      50% { 
        border-color: #dc2626;
        box-shadow: 0 0 25px rgba(220, 38, 38, 0.8), 0 0 40px rgba(220, 38, 38, 0.5);
      }
    }
    .alert-glow-orange {
      animation: alertGlowOrange 0.8s ease-in-out infinite;
    }
    @keyframes alertGlowOrange {
      0%, 100% { 
        border-color: #ea580c;
        box-shadow: 0 0 5px rgba(234, 88, 12, 0.3);
      }
      50% { 
        border-color: #f97316;
        box-shadow: 0 0 25px rgba(249, 115, 22, 0.8), 0 0 40px rgba(249, 115, 22, 0.5);
      }
    }
    .video-fade-in {
      animation: videoFadeIn 1s ease-out;
    }
    @keyframes videoFadeIn {
      from { opacity: 0; transform: scale(0.95); }
      to { opacity: 1; transform: scale(1); }
    }
    .video-fade-out {
      animation: videoFadeOut 0.8s ease-in forwards;
    }
    @keyframes videoFadeOut {
      from { opacity: 1; transform: scale(1); }
      to { opacity: 0; transform: scale(1.05); }
    }
    .gallery-slide-out-left {
      animation: gallerySlideOutLeft 0.8s cubic-bezier(0.68, -0.55, 0.265, 1.55) forwards;
    }
    @keyframes gallerySlideOutLeft {
      0% { 
        opacity: 1; 
        transform: translateX(0) scale(1) rotateY(0deg);
        filter: blur(0px) brightness(1);
      }
      100% { 
        opacity: 0; 
        transform: translateX(-120%) scale(0.7) rotateY(-45deg);
        filter: blur(4px) brightness(0.7);
      }
    }
    .gallery-slide-in-right {
      animation: gallerySlideInRight 1s cubic-bezier(0.34, 1.56, 0.64, 1) forwards;
    }
    @keyframes gallerySlideInRight {
      0% { 
        opacity: 0; 
        transform: translateX(120%) scale(0.7) rotateY(45deg);
        filter: blur(4px) brightness(1.3);
      }
      100% { 
        opacity: 1; 
        transform: translateX(0) scale(1) rotateY(0deg);
        filter: blur(0px) brightness(1);
      }
    }
    .gallery-zoom-pulse {
      animation: galleryZoomPulse 1.2s ease-in-out;
    }
    @keyframes galleryZoomPulse {
      0%, 100% { 
        transform: scale(1);
        box-shadow: 0 0 0 rgba(94, 234, 212, 0);
      }
      50% { 
        transform: scale(1.05);
        box-shadow: 0 0 30px rgba(94, 234, 212, 0.6);
      }
    }
    .sync-indicator {
      position: fixed;
      top: 50px;
      right: 10px;
      z-index: 1000;
      padding: 8px 12px;
      border-radius: 8px;
      font-size: 11px;
      font-weight: bold;
      display: flex;
      align-items: center;
      gap: 6px;
      transition: all 0.3s ease;
      background: linear-gradient(135deg, #10b981 0%, #059669 100%);
      color: #f0fdfa;
      box-shadow: 0 2px 8px rgba(16, 185, 129, 0.4);
    }
    .sync-indicator.syncing {
      background: linear-gradient(135deg, #fbbf24 0%, #f59e0b 100%);
      animation: pulse 1s ease-in-out infinite;
    }
    .sync-indicator.error {
      background: linear-gradient(135deg, #ef4444 0%, #dc2626 100%);
    }
    @keyframes pulse {
      0%, 100% { opacity: 1; }
      50% { opacity: 0.7; }
    }
  </style>
  <style>@view-transition { navigation: auto; }</style>
  <script src="/_sdk/data_sdk.js" type="text/javascript"></script>
 </head>
 <body class="h-full w-full honeycomb-bg" style="color: #78350f;"><!-- Sync Indicator -->
  <div id="sync-indicator" class="sync-indicator"><span id="sync-icon">🔄</span> <span id="sync-text">متصل</span>
  </div>
  <header class="sr-only">
   <h1>شاشة عرض مخزون العسل</h1>
  </header>
  <main class="h-full w-full"><!-- Login Screen -->
   <div id="login-screen" class="w-full h-full flex items-center justify-center">
    <div class="w-full max-w-md mx-auto px-6">
     <div class="rounded-2xl shadow-2xl p-8" style="background: linear-gradient(135deg, #054239 0%, #043830 50%, #032d27 100%); border: 3px solid rgba(94, 234, 212, 0.4);">
      <div class="text-center mb-8">
       <div class="text-5xl mb-4">
        🍯
       </div>
       <h2 class="text-3xl font-bold mb-2" style="color: #f0fdfa;">تسجيل الدخول</h2>
       <p class="text-sm" style="color: rgba(240, 253, 250, 0.7);">نظام إدارة مخزون العسل</p>
      </div>
      <form id="login-form" class="flex flex-col gap-4">
       <div class="flex flex-col gap-2"><label for="username" class="text-sm font-medium" style="color: #f0fdfa;">اسم المستخدم</label> <input type="text" id="username" required class="px-4 py-3 rounded-lg font-bold text-base" style="background: rgba(3, 45, 39, 0.6); color: #f0fdfa; border: 2px solid rgba(94, 234, 212, 0.3);" placeholder="أدخل اسم المستخدم">
       </div>
       <div class="flex flex-col gap-2"><label for="password" class="text-sm font-medium" style="color: #f0fdfa;">كلمة المرور</label> <input type="password" id="password" required class="px-4 py-3 rounded-lg font-bold text-base" style="background: rgba(3, 45, 39, 0.6); color: #f0fdfa; border: 2px solid rgba(94, 234, 212, 0.3);" placeholder="أدخل كلمة المرور">
       </div>
       <div id="login-error" class="hidden px-4 py-3 rounded-lg text-sm font-medium text-center" style="background: rgba(220, 38, 38, 0.2); color: #fca5a5; border: 2px solid rgba(220, 38, 38, 0.4);">
        اسم المستخدم أو كلمة المرور غير صحيح
       </div><button type="submit" class="px-6 py-3 rounded-lg font-bold text-lg transition-all duration-300 hover:scale-105" style="background: linear-gradient(135deg, #14b8a6 0%, #0d9488 100%); color: #f0fdfa;"> دخول </button>
      </form>
      <div class="mt-6 pt-6" style="border-top: 2px solid rgba(94, 234, 212, 0.2);">
       <p class="text-xs text-center mb-3" style="color: rgba(240, 253, 250, 0.6);">حسابات تجريبية:</p>
       <div class="flex flex-col gap-2 text-xs" style="color: rgba(240, 253, 250, 0.8);">
        <div class="flex items-center justify-between px-3 py-2 rounded" style="background: rgba(3, 45, 39, 0.4);"><span>👤 <strong>admin</strong> / admin123</span> <span class="text-[10px] px-2 py-1 rounded" style="background: rgba(94, 234, 212, 0.2); color: #5eead4;">مدير</span>
        </div>
        <div class="flex items-center justify-between px-3 py-2 rounded" style="background: rgba(3, 45, 39, 0.4);"><span>👁️ <strong>viewer</strong> / viewer123</span> <span class="text-[10px] px-2 py-1 rounded" style="background: rgba(251, 191, 36, 0.2); color: #fbbf24;">مشاهد</span>
        </div>
       </div>
      </div>
     </div>
    </div>
   </div><!-- Main App Screen -->
   <div id="app-screen" class="w-full h-full flex items-stretch justify-center hidden">
    <div class="app-wrapper w-full h-full max-w-7xl mx-auto px-1 py-2 flex flex-col gap-0.5 relative">
     <div class="honeycomb-mask absolute inset-0 pointer-events-none"></div><!-- User Info & Logout Button -->
     <div class="fixed top-2 right-2 z-50 flex items-center gap-1.5">
      <div class="px-2 py-1 rounded-full shadow-md" style="background: linear-gradient(135deg, #054239 0%, #043830 100%); border: 1px solid rgba(94, 234, 212, 0.4);">
       <div class="flex items-center gap-1"><span id="user-role-badge" class="text-[8px] font-bold px-1.5 py-0.5 rounded-full" style="background: rgba(94, 234, 212, 0.2); color: #5eead4;">مدير</span> <span id="user-name-display" class="text-[9px] font-bold" style="color: #f0fdfa;">admin</span>
       </div>
      </div><button id="logout-btn" class="w-7 h-7 rounded-full font-bold text-xs transition-all duration-300 shadow-md hover:shadow-lg hover:scale-110 flex items-center justify-center" style="background: linear-gradient(135deg, #ef4444 0%, #dc2626 100%); color: #fef2f2;" title="تسجيل الخروج" aria-label="تسجيل الخروج"> 🚪 </button>
     </div><!-- Control Panel Button --> <button id="toggle-control-panel" class="fixed top-2 left-2 z-50 w-7 h-7 rounded-full font-bold text-xs transition-all duration-300 shadow-md hover:shadow-lg hover:scale-110 flex items-center justify-center" style="background: linear-gradient(135deg, #14b8a6 0%, #0d9488 100%); color: #f0fdfa;" title="لوحة التحكم" aria-label="فتح لوحة التحكم"> ⚙️ </button> <!-- Center Alert Container -->
     <div id="center-alert-container" class="fixed inset-0 z-[200] hidden pointer-events-none" style="background: rgba(0, 0, 0, 0.75); backdrop-filter: blur(8px);">
     </div><!-- Control Panel Overlay -->
     <div id="control-panel-overlay" class="fixed inset-0 z-[100] hidden" style="background: rgba(0, 0, 0, 0.7); backdrop-filter: blur(4px);">
      <div class="w-full h-full overflow-y-auto p-6">
       <div class="max-w-6xl mx-auto">
        <div class="mb-6 flex items-center justify-between">
         <h2 class="text-3xl font-bold" style="color: #5eead4;">⚙️ لوحة التحكم بالمخزون</h2><button id="close-control-panel" class="px-4 py-2 rounded-lg font-bold transition-all duration-300 hover:scale-105" style="background: rgba(220, 38, 38, 0.2); color: #fca5a5; border: 2px solid rgba(220, 38, 38, 0.4);"> ✕ إغلاق </button>
        </div>
        <div class="flex flex-col gap-6"><!-- Variants Control Section -->
         <section class="rounded-xl p-6" style="background: rgba(5, 66, 57, 0.9); border: 2px solid rgba(94, 234, 212, 0.3);">
          <div class="flex items-center justify-between mb-4">
           <h3 class="text-xl font-bold" style="color: #f0fdfa;">🍯 إدارة الأصناف والكميات</h3><button id="add-variant-btn" class="px-4 py-2 rounded-lg font-bold text-sm transition-all duration-300 hover:scale-105" style="background: linear-gradient(135deg, #14b8a6 0%, #0d9488 100%); color: #f0fdfa;"> ➕ إضافة صنف جديد </button>
          </div>
          <div id="variants-control-list" class="flex flex-col gap-3 max-h-96 overflow-y-auto" style="scrollbar-width: thin; scrollbar-color: rgba(94, 234, 212, 0.3) rgba(3, 45, 39, 0.3);">
          </div>
         </section><!-- Quick Actions Section -->
         <section class="rounded-xl p-6" style="background: rgba(5, 66, 57, 0.9); border: 2px solid rgba(94, 234, 212, 0.3);">
          <h3 class="text-xl font-bold mb-4" style="color: #f0fdfa;">⚡ إجراءات سريعة</h3>
          <div class="grid grid-cols-3 gap-3"><button id="reset-all-btn" class="px-4 py-3 rounded-lg font-bold text-sm transition-all duration-300 hover:scale-105" style="background: linear-gradient(135deg, #fbbf24 0%, #f59e0b 100%); color: #78350f;"> 🔄 إعادة تعيين الكل </button> <button id="increase-all-btn" class="px-4 py-3 rounded-lg font-bold text-sm transition-all duration-300 hover:scale-105" style="background: linear-gradient(135deg, #10b981 0%, #059669 100%); color: #f0fdfa;"> ⬆️ زيادة الكل +10 </button> <button id="decrease-all-btn" class="px-4 py-3 rounded-lg font-bold text-sm transition-all duration-300 hover:scale-105" style="background: linear-gradient(135deg, #ef4444 0%, #dc2626 100%); color: #fef2f2;"> ⬇️ تقليل الكل -10 </button>
          </div>
         </section><!-- Alert Boxes Section -->
         <section class="rounded-xl p-6" style="background: rgba(5, 66, 57, 0.9); border: 2px solid rgba(94, 234, 212, 0.3);">
          <h3 class="text-xl font-bold mb-4" style="color: #f0fdfa;">⚠️ إدارة مربعات التنويه</h3>
          <div class="grid grid-cols-2 gap-4"><!-- Orange Alert -->
           <div class="flex flex-col gap-3"><label class="text-sm font-medium" style="color: #f0fdfa;">🟠 التنويه البرتقالي</label> <textarea id="orange-alert-text" placeholder="أدخل نص التنويه البرتقالي..." rows="3" class="px-4 py-2 rounded-lg font-bold text-sm resize-none" style="background: rgba(3, 45, 39, 0.6); color: #f0fdfa; border: 2px solid rgba(94, 234, 212, 0.3);"></textarea>
            <div class="flex gap-2"><button id="show-orange-alert-btn" class="flex-1 px-4 py-2 rounded-lg font-bold text-sm transition-all duration-300 hover:scale-105" style="background: linear-gradient(135deg, #f97316 0%, #ea580c 100%); color: #fff7ed;"> عرض </button> <button id="hide-orange-alert-btn" class="flex-1 px-4 py-2 rounded-lg font-bold text-sm transition-all duration-300 hover:scale-105" style="background: rgba(120, 53, 15, 0.3); color: rgba(240, 253, 250, 0.7); border: 2px solid rgba(94, 234, 212, 0.2);"> إخفاء </button>
            </div>
           </div><!-- Red Alert -->
           <div class="flex flex-col gap-3"><label class="text-sm font-medium" style="color: #f0fdfa;">🔴 التنويه الأحمر</label> <textarea id="red-alert-text" placeholder="أدخل نص التنويه الأحمر..." rows="3" class="px-4 py-2 rounded-lg font-bold text-sm resize-none" style="background: rgba(3, 45, 39, 0.6); color: #f0fdfa; border: 2px solid rgba(94, 234, 212, 0.3);"></textarea>
            <div class="flex gap-2"><button id="show-red-alert-btn" class="flex-1 px-4 py-2 rounded-lg font-bold text-sm transition-all duration-300 hover:scale-105" style="background: linear-gradient(135deg, #ef4444 0%, #dc2626 100%); color: #fef2f2;"> عرض </button> <button id="hide-red-alert-btn" class="flex-1 px-4 py-2 rounded-lg font-bold text-sm transition-all duration-300 hover:scale-105" style="background: rgba(120, 53, 15, 0.3); color: rgba(240, 253, 250, 0.7); border: 2px solid rgba(94, 234, 212, 0.2);"> إخفاء </button>
            </div>
           </div>
          </div>
         </section><!-- Manual Comment Control Section -->
         <section class="rounded-xl p-6" style="background: rgba(5, 66, 57, 0.9); border: 2px solid rgba(94, 234, 212, 0.3);">
          <h3 class="text-xl font-bold mb-4" style="color: #f0fdfa;">💬 إضافة تعليق يدوي</h3>
          <div class="flex flex-col gap-3">
           <div class="flex flex-col gap-2"><label class="text-sm font-medium" style="color: #f0fdfa;">اسم المعلق</label> <input type="text" id="manual-comment-name" placeholder="أدخل اسم المعلق..." class="px-4 py-2 rounded-lg font-bold text-sm" style="background: rgba(3, 45, 39, 0.6); color: #f0fdfa; border: 2px solid rgba(94, 234, 212, 0.3);">
           </div>
           <div class="flex flex-col gap-2"><label class="text-sm font-medium" style="color: #f0fdfa;">نص التعليق</label> <textarea id="manual-comment-text" placeholder="أدخل نص التعليق..." rows="3" class="px-4 py-2 rounded-lg font-bold text-sm resize-none" style="background: rgba(3, 45, 39, 0.6); color: #f0fdfa; border: 2px solid rgba(94, 234, 212, 0.3);"></textarea>
           </div><button id="add-manual-comment-btn" class="px-6 py-3 rounded-lg font-bold text-sm transition-all duration-300 hover:scale-105" style="background: linear-gradient(135deg, #14b8a6 0%, #0d9488 100%); color: #f0fdfa;"> ✨ إضافة التعليق وعرضه الآن </button>
          </div>
         </section><!-- News Ticker Control Section -->
         <section class="rounded-xl p-6" style="background: rgba(5, 66, 57, 0.9); border: 2px solid rgba(94, 234, 212, 0.3);">
          <div class="flex items-center justify-between mb-4">
           <h3 class="text-xl font-bold" style="color: #f0fdfa;">📢 إدارة الشريط الإخباري</h3><button id="add-news-btn" class="px-4 py-2 rounded-lg font-bold text-sm transition-all duration-300 hover:scale-105" style="background: linear-gradient(135deg, #14b8a6 0%, #0d9488 100%); color: #f0fdfa;"> ➕ إضافة خبر </button>
          </div>
          <div id="news-control-list" class="flex flex-col gap-3 max-h-96 overflow-y-auto" style="scrollbar-width: thin; scrollbar-color: rgba(94, 234, 212, 0.3) rgba(3, 45, 39, 0.3);">
          </div>
         </section>
        </div>
       </div>
      </div>
     </div><!-- Header -->
     <section class="relative z-10">
      <div class="flex flex-col gap-0 fade-in">
       <div class="flex items-center justify-between gap-2"><!-- Left: Empty spacer for balance -->
        <div class="flex-1"></div><!-- Center: Main Title -->
        <div class="flex flex-col gap-0 items-center text-center">
         <h2 id="main-title" class="tracking-wide font-bold text-2xl" style="color: #054239;">عرض التحرير</h2>
         <p id="subtitle" class="text-sm" style="color: #054239;">احتفال يستحقه الشعب السوري</p>
        </div><!-- Right: Live Badge -->
        <div class="flex items-center gap-1 flex-1 justify-end">
         <div class="px-2 py-1 rounded-lg bg-amber-400/20 border-2 flex items-center gap-1" style="border-color: #054239;"><span class="relative inline-flex h-2.5 w-2.5"> <span class="pulse-ring absolute inline-flex h-full w-full rounded-full bg-emerald-500 opacity-75"></span> <span class="relative inline-flex rounded-full h-2.5 w-2.5 bg-emerald-500 shadow-[0_0_8px_rgba(16,185,129,0.9)]"></span> </span> <span class="text-[10px] font-bold tracking-wide" style="color: #054239;"> مباشر </span>
         </div>
        </div>
       </div>
      </div>
     </section><!-- Top Bar: Main Inventory Display -->
     <section class="relative z-10">
      <article class="relative rounded-md overflow-hidden px-2 py-1 slide-up shadow-lg" style="background: linear-gradient(135deg, #054239 0%, #043830 50%, #032d27 100%); border: 1px solid rgba(94, 234, 212, 0.4);" aria-label="المخزون المتبقي">
       <div class="absolute -top-12 -right-12 w-36 h-36 rounded-full bg-amber-300/35 blur-2xl pointer-events-none"></div>
       <div class="absolute -bottom-10 -left-10 w-32 h-32 rounded-full bg-amber-400/25 blur-2xl pointer-events-none"></div>
       <div class="relative flex items-center justify-between gap-2"><!-- Right: Label & Icon -->
        <div class="flex items-center gap-1">
         <div class="relative w-5 h-5 jar-pulse">
          <svg viewbox="0 0 80 80" class="w-full h-full drop-shadow-lg"><defs>
            <lineargradient id="jarGradient" x1="0" y1="0" x2="0" y2="1">
             <stop offset="0%" stop-color="#FDE68A" />
             <stop offset="45%" stop-color="#FBBF24" />
             <stop offset="100%" stop-color="#92400E" />
            </lineargradient>
            <lineargradient id="jarGlass" x1="0" y1="0" x2="1" y2="1">
             <stop offset="0%" stop-color="#F9FAFB" stop-opacity="0.3" />
             <stop offset="60%" stop-color="#FBBF24" stop-opacity="0.08" />
             <stop offset="100%" stop-color="#000000" stop-opacity="0.0" />
            </lineargradient>
           </defs> <ellipse cx="40" cy="68" rx="22" ry="7" fill="#000000" opacity="0.4" /> <rect x="18" y="24" width="44" height="38" rx="11" fill="url(#jarGradient)" /> <path d="M22 26h14c9 0 16 7 16 16v20c0 0-9-5-18-5-8 0-12 3-12 3V26z" fill="url(#jarGlass)" /> <rect x="24" y="18" width="32" height="9" rx="4" fill="#78350F" /> <rect x="26" y="16" width="28" height="4" rx="2" fill="#FBBF24" /> <path d="M30 34c4 0 5 4 5 7s-1 7-5 7-5-4-5-7 1-7 5-7z" fill="#FBBF24" opacity="0.95" />
          </svg>
         </div>
         <div class="flex flex-col gap-0">
          <p id="remaining-label" class="text-[7px] tracking-[0.2em] uppercase font-semibold" style="color: #f0fdfa;">المخزون المتبقي</p>
          <p class="text-[6px] leading-tight" style="color: rgba(240, 253, 250, 0.8);">العد التنازلي المباشر</p>
         </div>
        </div><!-- Center: Giant Number -->
        <div class="flex items-baseline gap-0.5"><span id="remaining-value" class="smooth-number font-bold text-3xl leading-none tracking-tight" style="color: #f0fdfa; text-shadow: 0 0 12px rgba(94, 234, 212, 0.5)" aria-live="polite" aria-atomic="true"> 160 </span> <span class="text-sm font-medium" style="color: rgba(240, 253, 250, 0.8);"> كغ </span>
        </div><!-- Right: Progress & Status -->
        <div class="flex-1 flex flex-col gap-0.5">
         <div class="flex items-center justify-between text-[7px] font-medium" style="color: #f0fdfa;"><span>مستوى المخزون</span>
          <div class="flex items-center gap-0.5"><span id="status-label" class="px-1 py-0.5 rounded-full bg-emerald-500/25 text-[6px] border border-emerald-600/50 font-semibold" style="color: #6ee7b7;"> مستقر </span>
          </div>
         </div>
         <div class="w-full h-1.5 rounded-full overflow-hidden shadow-inner" style="background-color: rgba(3, 45, 39, 0.6);">
          <div id="progress-bar" class="h-full rounded-full shadow-lg" style="width: 100%; transition: width 0.6s ease-out, background 0.6s ease-out; background: linear-gradient(90deg, #5eead4 0%, #14b8a6 100%);"></div>
         </div>
         <div class="flex items-center justify-start text-[6px]" style="color: rgba(240, 253, 250, 0.75);"><span class="inline-flex items-center gap-0.5"> <span id="change-percent" class="text-[10px] font-bold">100%</span> <span id="change-arrow" class="text-[8px]" style="color: #6ee7b7;">◆</span> </span>
         </div>
        </div>
       </div>
      </article>
     </section><!-- Main Content Grid: 3 Columns -->
     <section class="relative z-10 grid grid-cols-12 gap-3 flex-1 min-h-0" style="margin-bottom: 50px; max-height: calc(100% - 140px);"><!-- Right Column: Variants Table (كمية المخزون) -->
      <aside class="col-span-3 flex flex-col">
       <section class="rounded-lg px-3 py-2 flex flex-col gap-2 slide-up shadow-lg flex-1 min-h-0" style="animation-delay: 0.05s; background: linear-gradient(135deg, #054239 0%, #043830 50%, #032d27 100%); border: 2px solid rgba(94, 234, 212, 0.4);" aria-label="تفصيل المخزون">
        <div class="flex items-center justify-between gap-1.5 flex-shrink-0">
         <p class="text-[10px] tracking-[0.2em] uppercase font-bold" style="color: #f0fdfa;">كمية المخزون</p>
        </div>
        <div id="variants-list" class="flex-1 flex flex-col gap-1.5 overflow-y-auto min-h-0" style="scrollbar-width: thin; scrollbar-color: rgba(94, 234, 212, 0.3) rgba(3, 45, 39, 0.3);">
        </div>
        <div class="pt-2 text-[10px] font-medium flex-shrink-0" style="border-top: 2px solid rgba(94, 234, 212, 0.3); color: #f0fdfa;">
         <div class="flex items-center justify-between"><span>المجموع:</span> <span id="variants-total" class="text-sm font-bold">160 كغ</span>
         </div>
        </div>
       </section>
      </aside><!-- Middle Column: Screen Share & Videos (مشاركة الشاشة والفيديوهات) -->
      <aside class="col-span-7 flex flex-col">
       <article class="rounded-lg px-4 py-3 flex flex-col gap-3 slide-up shadow-lg flex-1 min-h-0" style="animation-delay: 0.1s; background: linear-gradient(135deg, #054239 0%, #043830 50%, #032d27 100%); border: 2px solid rgba(94, 234, 212, 0.4);" aria-label="مساحة مشاركة الشاشة والفيديوهات">
        <div class="flex items-center justify-between gap-2 flex-shrink-0">
         <div class="flex items-center gap-2"><button id="mode-screen-share" class="mode-btn px-3 py-1.5 rounded-lg text-[9px] font-bold transition-all duration-300" style="background: linear-gradient(135deg, #14b8a6 0%, #0d9488 100%); color: #f0fdfa;"> 🖥️ مشاركة الشاشة </button> <button id="mode-videos" class="mode-btn px-3 py-1.5 rounded-lg text-[9px] font-bold transition-all duration-300" style="background: rgba(94, 234, 212, 0.2); color: rgba(240, 253, 250, 0.7);"> 🎬 الفيديوهات </button> <button id="mode-images" class="mode-btn px-3 py-1.5 rounded-lg text-[9px] font-bold transition-all duration-300" style="background: rgba(94, 234, 212, 0.2); color: rgba(240, 253, 250, 0.7);"> 🖼️ الصور </button>
         </div>
         <div id="screen-share-controls" class="items-center gap-2"><button id="toggle-screen-share" class="px-4 py-2 rounded-lg text-[10px] font-bold transition-all duration-300 hover:scale-105" style="background: linear-gradient(135deg, #14b8a6 0%, #0d9488 100%); color: #f0fdfa;"> بدء المشاركة </button>
         </div>
         <div id="videos-controls" class="items-center gap-2"><label class="px-3 py-1.5 rounded-lg text-[9px] font-bold transition-all duration-300 hover:scale-105 cursor-pointer" style="background: linear-gradient(135deg, #14b8a6 0%, #0d9488 100%); color: #f0fdfa;"> 📤 رفع فيديو <input type="file" id="video-upload-input" accept="video/*" class="hidden" multiple> </label> <button id="toggle-video-playback" class="px-3 py-1.5 rounded-lg text-[9px] font-bold transition-all duration-300 hover:scale-105" style="background: linear-gradient(135deg, #10b981 0%, #059669 100%); color: #f0fdfa;"> ▶️ تشغيل </button>
         </div>
         <div id="images-controls" class="items-center gap-2"><label class="px-3 py-1.5 rounded-lg text-[9px] font-bold transition-all duration-300 hover:scale-105 cursor-pointer" style="background: linear-gradient(135deg, #14b8a6 0%, #0d9488 100%); color: #f0fdfa;"> 📤 رفع صور <input type="file" id="display-image-upload-input" accept="image/*" class="hidden" multiple> </label> <button id="toggle-image-slideshow" class="px-3 py-1.5 rounded-lg text-[9px] font-bold transition-all duration-300 hover:scale-105" style="background: linear-gradient(135deg, #10b981 0%, #059669 100%); color: #f0fdfa;"> ▶️ تشغيل </button>
         </div>
        </div>
        <div id="screen-share-display" class="flex-1 rounded-lg overflow-hidden flex items-center justify-center min-h-0" style="background: rgba(3, 45, 39, 0.6); border: 2px dashed rgba(94, 234, 212, 0.3); position: relative;">
         <p class="text-base" style="color: rgba(240, 253, 250, 0.6);">انقر "بدء المشاركة" لعرض الشاشة</p>
        </div>
        <div id="videos-display" class="hidden flex-1 rounded-lg overflow-hidden min-h-0 relative" style="background: rgba(3, 45, 39, 0.6); border: 2px solid rgba(94, 234, 212, 0.3); position: relative;">
         <div id="video-player-container" class="w-full h-full flex items-center justify-center" style="position: absolute; top: 0; left: 0; right: 0; bottom: 0;">
          <p class="text-base text-center px-4" style="color: rgba(240, 253, 250, 0.6);">ارفع فيديوهات لعرضها بانتقالات احترافية</p>
         </div>
         <div id="video-progress-bar" class="hidden absolute bottom-0 left-0 right-0 h-1" style="background: rgba(3, 45, 39, 0.8);">
          <div id="video-progress-fill" class="h-full transition-all duration-300" style="width: 0%; background: linear-gradient(90deg, #14b8a6 0%, #5eead4 100%);"></div>
         </div>
         <div id="video-info-overlay" class="hidden absolute top-2 left-2 right-2 px-3 py-2 rounded-lg backdrop-blur-sm" style="background: rgba(5, 66, 57, 0.9); border: 1px solid rgba(94, 234, 212, 0.4);">
          <div class="flex items-center justify-between"><span id="video-current-name" class="text-xs font-bold" style="color: #f0fdfa;">الفيديو 1</span> <span id="video-counter" class="text-[10px]" style="color: rgba(240, 253, 250, 0.7);">1 / 3</span>
          </div>
         </div>
        </div>
        <div id="images-display" class="hidden flex-1 rounded-lg overflow-hidden min-h-0 relative" style="background: rgba(3, 45, 39, 0.6); border: 2px solid rgba(94, 234, 212, 0.3); position: relative;">
         <div id="image-slideshow-container" class="w-full h-full flex items-center justify-center" style="position: absolute; top: 0; left: 0; right: 0; bottom: 0;">
          <p class="text-base text-center px-4" style="color: rgba(240, 253, 250, 0.6);">ارفع صور لعرضها بشكل احترافي</p>
         </div>
         <div id="image-info-overlay" class="hidden absolute top-2 left-2 right-2 px-3 py-2 rounded-lg backdrop-blur-sm" style="background: rgba(5, 66, 57, 0.9); border: 1px solid rgba(94, 234, 212, 0.4);">
          <div class="flex items-center justify-between"><span id="image-current-name" class="text-xs font-bold" style="color: #f0fdfa;">الصورة 1</span> <span id="image-counter" class="text-[10px]" style="color: rgba(240, 253, 250, 0.7);">1 / 3</span>
          </div>
         </div>
        </div>
       </article>
      </aside><!-- Left Column: Image Gallery (معرض الصور) - Vertical -->
      <aside class="col-span-2 row-span-2 flex flex-col">
       <article class="rounded-lg px-3 py-2 flex flex-col gap-2 slide-up shadow-lg flex-1 min-h-0" style="animation-delay: 0.15s; background: linear-gradient(135deg, #054239 0%, #043830 50%, #032d27 100%); border: 2px solid rgba(94, 234, 212, 0.4);" aria-label="مساحة رفع الصور">
        <div id="gallery-header" class="flex items-center justify-between gap-2 flex-shrink-0">
         <p id="gallery-title" class="text-[10px] tracking-[0.2em] uppercase font-bold" style="color: #f0fdfa;">📸 معرض الصور</p><label id="gallery-upload-btn" class="px-3 py-1.5 rounded-lg text-[9px] font-bold transition-all duration-300 hover:scale-105 cursor-pointer" style="background: linear-gradient(135deg, #14b8a6 0%, #0d9488 100%); color: #f0fdfa;"> رفع <input type="file" id="image-upload-input" accept="image/*" class="hidden" multiple> </label>
        </div>
        <div id="images-gallery" class="flex-1 flex flex-col gap-2 overflow-y-auto min-h-0" style="scrollbar-width: thin; scrollbar-color: rgba(94, 234, 212, 0.3) rgba(3, 45, 39, 0.3);">
         <div class="rounded-lg flex items-center justify-center h-full" style="background: rgba(3, 45, 39, 0.6); border: 2px dashed rgba(94, 234, 212, 0.3);">
          <p class="text-[10px] text-center px-2" style="color: rgba(240, 253, 250, 0.6);">ارفع صور المنتجات</p>
         </div>
        </div>
       </article>
      </aside>
     </section><!-- Comments Notification Area -->
     <div id="comments-container" class="fixed left-8 z-50 flex flex-col gap-3 pointer-events-none" style="max-width: 380px; bottom: 56px;">
     </div><!-- Hearts Container -->
     <div id="hearts-container" class="fixed bottom-0 z-40 pointer-events-none" style="width: 200px; height: 100%; left: calc(50% - 400px);">
     </div><!-- News Ticker -->
     <div class="fixed bottom-0 left-0 right-0 z-50 overflow-hidden shadow-2xl" style="background: linear-gradient(135deg, #054239 0%, #043830 50%, #032d27 100%); border-top: 3px solid rgba(94, 234, 212, 0.5); height: 40px;">
      <div class="h-full flex items-center px-4">
       <div class="flex-shrink-0 flex items-center gap-2 ml-4"><span class="text-lg">📢</span> <span class="text-sm font-bold tracking-wide" style="color: #fbbf24;">أخبار:</span>
       </div>
       <div class="flex-1 overflow-hidden relative">
        <div id="news-ticker-content" class="flex items-center gap-8 whitespace-nowrap" style="animation: scroll-news 60s linear infinite;">
        </div>
       </div>
      </div>
     </div>
    </div>
   </div>
  </main>
  <script>
    // ============================================
    // FIREBASE CONFIGURATION - REPLACE WITH YOUR OWN
    // ============================================
    const firebaseConfig = {
      apiKey: "AIzaSyDEMOKEY123456789",
      authDomain: "your-project.firebaseapp.com",
      databaseURL: "https://your-project-default-rtdb.firebaseio.com",
      projectId: "your-project",
      storageBucket: "your-project.appspot.com",
      messagingSenderId: "123456789",
      appId: "1:123456789:web:abc123def456"
    };

    // Initialize Firebase
    let database = null;
    let syncEnabled = false;

    try {
      firebase.initializeApp(firebaseConfig);
      database = firebase.database();
      syncEnabled = true;
      updateSyncIndicator('connected', 'متصل', '✅');
    } catch (error) {
      console.error("Firebase initialization error:", error);
      updateSyncIndicator('error', 'غير متصل', '❌');
    }

    // Sync Indicator Functions
    function updateSyncIndicator(status, text, icon) {
      const indicator = document.getElementById('sync-indicator');
      const iconEl = document.getElementById('sync-icon');
      const textEl = document.getElementById('sync-text');
      
      if (!indicator || !iconEl || !textEl) return;
      
      indicator.className = 'sync-indicator';
      
      if (status === 'syncing') {
        indicator.classList.add('syncing');
      } else if (status === 'error') {
        indicator.classList.add('error');
      }
      
      iconEl.textContent = icon;
      textEl.textContent = text;
    }

    // Sync Functions
    function syncToFirebase(path, data) {
      if (!syncEnabled || !database) return;
      
      updateSyncIndicator('syncing', 'جاري المزامنة...', '🔄');
      
      database.ref(path).set(data)
        .then(() => {
          updateSyncIndicator('connected', 'تم الحفظ', '✅');
          setTimeout(() => {
            updateSyncIndicator('connected', 'متصل', '✅');
          }, 2000);
        })
        .catch((error) => {
          console.error("Sync error:", error);
          updateSyncIndicator('error', 'خطأ في الحفظ', '❌');
        });
    }

    function listenToFirebase(path, callback) {
      if (!syncEnabled || !database) return;
      
      database.ref(path).on('value', (snapshot) => {
        const data = snapshot.val();
        if (data) {
          callback(data);
        }
      });
    }

    // User Authentication System
    const users = {
      admin: { password: "admin123", role: "admin" },
      viewer: { password: "viewer123", role: "viewer" }
    };

    let currentUser = null;

    const defaultConfig = {
      main_title: "عرض التحرير",
      subtitle: "احتفال يستحقه الشعب السوري",
      remaining_label: "المخزون المتبقي",
      background_color: "#edebe0",
      surface_color: "#054239",
      text_color: "#f0fdfa",
      primary_action_color: "#fbbf24",
      secondary_action_color: "#78350f",
      font_family: "Noto Naskh Arabic",
      font_size: 16
    };

    const refs = {
      mainTitle: document.getElementById("main-title"),
      subtitle: document.getElementById("subtitle"),
      remainingLabel: document.getElementById("remaining-label"),
      remainingValue: document.getElementById("remaining-value"),
      progressBar: document.getElementById("progress-bar"),
      statusLabel: document.getElementById("status-label"),
      changeArrow: document.getElementById("change-arrow"),
      changePercent: document.getElementById("change-percent"),
      variantsList: document.getElementById("variants-list"),
      variantsTotal: document.getElementById("variants-total")
    };

    let currentQty = 160;
    let openingQty = 150;
    let variants = [
      { id: 1, name: "عسل السدر", quantity: 10, maxQuantity: 10 },
      { id: 2, name: "عسل جبلي", quantity: 20, maxQuantity: 20 },
      { id: 3, name: "عسل الصنوبر", quantity: 20, maxQuantity: 20 },
      { id: 4, name: "عسل الحمضيات", quantity: 10, maxQuantity: 10 },
      { id: 5, name: "عسل حبة البركة", quantity: 15, maxQuantity: 15 },
      { id: 6, name: "عسل الشوكيات", quantity: 10, maxQuantity: 10 },
      { id: 7, name: "عسل اللافندر", quantity: 10, maxQuantity: 10 },
      { id: 8, name: "عسل أبيض", quantity: 10, maxQuantity: 10 },
      { id: 9, name: "عسل الجيجان", quantity: 10, maxQuantity: 10 },
      { id: 10, name: "خلطة ملكية", quantity: 7, maxQuantity: 7 },
      { id: 11, name: "خلطة المتزوجين", quantity: 7, maxQuantity: 7 },
      { id: 12, name: "خلطة المناعة", quantity: 7, maxQuantity: 7 },
      { id: 13, name: "عسل ملكي", quantity: 7, maxQuantity: 7 },
      { id: 14, name: "خلطة فاخرة", quantity: 7, maxQuantity: 7 }
    ];

    const defaultComments = [
      { name: "محمد الأحمد", text: "شكراً على الخدمة المميزة والاهتمام الرائع" },
      { name: "ليلى حسن", text: "المنتج رائع وجودة فوق الممتاز ما شاء الله" },
      { name: "عمر خليل", text: "العرض خيالي استفدت كثير منه" },
      { name: "رنا يوسف", text: "طلبت العرض ووصلني بسرعة شكرا لكم" },
      { name: "ياسر المعلي", text: "انصحكم به بقوة تجربة ممتازة" },
      { name: "سارة محمود", text: "انتم بعسل عين مميزين دائما ما تقصرون" },
      { name: "بشار ابراهيم", text: "الخدمة سريعة والمنتج اروع من المتوقع" },
      { name: "نور عبدالله", text: "عرض مغري جدا طلبته وما ندمت" },
      { name: "فادي سليمان", text: "شكرا لكم على الجودة العالية والسعر المناسب" },
      { name: "مايا رمضان", text: "منتج رائع ويستحق التجربة انصح الجميع" }
    ];

    let newsItems = [
      { id: 1, text: "أحرّ التهاني للشعب السوري بمناسبة ذكرى التحرير الأولى، ذكرى تتجدد فيها الإرادة والعزيمة" },
      { id: 2, text: "🍯" },
      { id: 3, text: "يوم يحمل رمزاً لانتصار الصمود ووحدة أبناء الوطن رغم التحديات" },
      { id: 4, text: "🎉" },
      { id: 5, text: "نسأل الله أن تكون هذه المناسبة بداية مرحلة أكثر أمناً وازدهاراً لسوريا وشعبها" },
      { id: 6, text: "🕐" },
      { id: 7, text: "عسل عين منذ 2019 يمنحك نتائج علاجية حقيقية، والكمية محدودة جداً" },
      { id: 8, text: "✨" }
    ];

    // Setup Firebase Listeners (for viewers)
    if (syncEnabled) {
      listenToFirebase('variants', (data) => {
        if (currentUser && currentUser.role === 'viewer') {
          variants = data;
          updateVariantsDisplay();
        }
      });

      listenToFirebase('news', (data) => {
        if (currentUser && currentUser.role === 'viewer') {
          newsItems = data;
          updateNewsTicker();
        }
      });

      listenToFirebase('alerts/orange', (data) => {
        if (currentUser && currentUser.role === 'viewer' && data && data.show) {
          showOrangeAlert(data.text);
        }
      });

      listenToFirebase('alerts/red', (data) => {
        if (currentUser && currentUser.role === 'viewer' && data && data.show) {
          showRedAlert(data.text);
        }
      });

      listenToFirebase('comments/manual', (data) => {
        if (currentUser && currentUser.role === 'viewer' && data && data.show) {
          showManualComment(data.name, data.text);
          database.ref('comments/manual/show').set(false);
        }
      });
    }

    // Alert System
    let currentOrangeAlert = null;
    let currentRedAlert = null;
    const centerAlertContainer = document.getElementById("center-alert-container");

    function createAlertSound() {
      try {
        const audioContext = new (window.AudioContext || window.webkitAudioContext)();
        const oscillator = audioContext.createOscillator();
        const gainNode = audioContext.createGain();
        
        oscillator.connect(gainNode);
        gainNode.connect(audioContext.destination);
        
        oscillator.frequency.setValueAtTime(800, audioContext.currentTime);
        oscillator.frequency.exponentialRampToValueAtTime(400, audioContext.currentTime + 0.1);
        oscillator.frequency.exponentialRampToValueAtTime(600, audioContext.currentTime + 0.2);
        
        gainNode.gain.setValueAtTime(0.3, audioContext.currentTime);
        gainNode.gain.exponentialRampToValueAtTime(0.01, audioContext.currentTime + 0.3);
        
        oscillator.start(audioContext.currentTime);
        oscillator.stop(audioContext.currentTime + 0.3);
      } catch (e) {
        console.log("Audio not available");
      }
    }

    function showOrangeAlert(text) {
      hideOrangeAlert();
      createAlertSound();
      
      centerAlertContainer.classList.remove("hidden");
      centerAlertContainer.style.pointerEvents = "auto";
      
      const alertEl = document.createElement("div");
      alertEl.className = "center-alert-show alert-glow-orange rounded-2xl shadow-2xl p-8 max-w-lg mx-auto";
      alertEl.style.cssText = "position: absolute; top: 50%; left: 50%; transform: translate(-50%, -50%); background: linear-gradient(135deg, #f97316 0%, #ea580c 100%); border: 4px solid #ea580c;";
      alertEl.innerHTML = `
        <div class="flex flex-col items-center gap-6 text-center">
          <div class="text-6xl">⚠️</div>
          <div class="flex flex-col gap-3">
            <p class="text-2xl font-bold leading-relaxed" style="color: #fff7ed;">
              ${text}
            </p>
          </div>
          <button class="close-orange-alert px-8 py-3 rounded-xl font-bold text-base transition-all duration-300 hover:scale-105" style="background: rgba(255, 255, 255, 0.2); color: #fff7ed;">
            حسناً
          </button>
        </div>
      `;
      
      centerAlertContainer.appendChild(alertEl);
      currentOrangeAlert = alertEl;
      
      alertEl.querySelector(".close-orange-alert").addEventListener("click", hideOrangeAlert);
      
      setTimeout(() => {
        hideOrangeAlert();
      }, 8000);
    }

    function hideOrangeAlert() {
      if (currentOrangeAlert) {
        currentOrangeAlert.classList.remove("center-alert-show");
        currentOrangeAlert.classList.add("center-alert-hide");
        setTimeout(() => {
          if (currentOrangeAlert) {
            currentOrangeAlert.remove();
            currentOrangeAlert = null;
          }
          if (!currentRedAlert) {
            centerAlertContainer.classList.add("hidden");
            centerAlertContainer.style.pointerEvents = "none";
          }
        }, 400);
      }
    }

    function showRedAlert(text) {
      hideRedAlert();
      createAlertSound();
      
      centerAlertContainer.classList.remove("hidden");
      centerAlertContainer.style.pointerEvents = "auto";
      
      const alertEl = document.createElement("div");
      alertEl.className = "center-alert-show alert-glow-red rounded-2xl shadow-2xl p-8 max-w-lg mx-auto";
      alertEl.style.cssText = "position: absolute; top: 50%; left: 50%; transform: translate(-50%, -50%); background: linear-gradient(135deg, #6b1f2a 0%, #5a1922 50%, #4a131a 100%); border: 4px solid #6b1f2a;";
      alertEl.innerHTML = `
        <div class="flex flex-col items-center gap-6 text-center">
          <div class="text-6xl">🚨</div>
          <div class="flex flex-col gap-3">
            <p class="text-2xl font-bold leading-relaxed" style="color: #fef2f2;">
              ${text}
            </p>
          </div>
          <button class="close-red-alert px-8 py-3 rounded-xl font-bold text-base transition-all duration-300 hover:scale-105" style="background: rgba(255, 255, 255, 0.2); color: #fef2f2;">
            حسناً
          </button>
        </div>
      `;
      
      centerAlertContainer.appendChild(alertEl);
      currentRedAlert = alertEl;
      
      alertEl.querySelector(".close-red-alert").addEventListener("click", hideRedAlert);
      
      setTimeout(() => {
        hideRedAlert();
      }, 8000);
    }

    function hideRedAlert() {
      if (currentRedAlert) {
        currentRedAlert.classList.remove("center-alert-show");
        currentRedAlert.classList.add("center-alert-hide");
        setTimeout(() => {
          if (currentRedAlert) {
            currentRedAlert.remove();
            currentRedAlert = null;
          }
          if (!currentOrangeAlert) {
            centerAlertContainer.classList.add("hidden");
            centerAlertContainer.style.pointerEvents = "none";
          }
        }, 400);
      }
    }

    function showCenterAlert(title, message, icon) {
      if (title.includes("نفاد")) {
        showRedAlert(message);
      } else {
        showOrangeAlert(message);
      }
    }

    // Alert Control Buttons
    document.getElementById("show-orange-alert-btn").addEventListener("click", () => {
      const text = document.getElementById("orange-alert-text").value.trim();
      if (text) {
        showOrangeAlert(text);
        if (currentUser && currentUser.role === 'admin') {
          syncToFirebase('alerts/orange', { text: text, show: true });
        }
      }
    });

    document.getElementById("hide-orange-alert-btn").addEventListener("click", () => {
      hideOrangeAlert();
      if (currentUser && currentUser.role === 'admin') {
        syncToFirebase('alerts/orange', { text: "", show: false });
      }
    });

    document.getElementById("show-red-alert-btn").addEventListener("click", () => {
      const text = document.getElementById("red-alert-text").value.trim();
      if (text) {
        showRedAlert(text);
        if (currentUser && currentUser.role === 'admin') {
          syncToFirebase('alerts/red', { text: text, show: true });
        }
      }
    });

    document.getElementById("hide-red-alert-btn").addEventListener("click", () => {
      hideRedAlert();
      if (currentUser && currentUser.role === 'admin') {
        syncToFirebase('alerts/red', { text: "", show: false });
      }
    });

    // Control Panel Management
    const controlPanelOverlay = document.getElementById("control-panel-overlay");
    const closeControlPanelBtn = document.getElementById("close-control-panel");

    function formatNumber(value) {
      return value.toLocaleString("en-US");
    }

    function calculateVariantsTotal() {
      return variants.reduce((sum, v) => sum + v.quantity, 0);
    }

    function checkInventoryAlerts() {
      const lowStockVariants = variants.filter(v => v.quantity >= 1 && v.quantity <= 2);
      const outOfStockVariants = variants.filter(v => v.quantity === 0);
      
      if (outOfStockVariants.length > 0) {
        const variantName = outOfStockVariants[0].name;
        showCenterAlert(
          "⚠️ نفاد الكمية",
          `للأسف نفدت الكمية المخصصة للعرض من ${variantName}`,
          "😔"
        );
      } else if (lowStockVariants.length > 0) {
        const variant = lowStockVariants[0];
        showCenterAlert(
          "⚠️ تنبيه مخزون منخفض",
          `${variant.name} - بقي ${variant.quantity} كيلو على نفاد الكمية`,
          "⚡"
        );
      }
    }

    function updateVariantsDisplay() {
      const total = calculateVariantsTotal();
      refs.variantsTotal.textContent = formatNumber(Math.round(total)) + " كغ";
      
      refs.variantsList.innerHTML = "";
      
      const gradientColors = [
        ["#FDE68A", "#FBBF24", "#B45309"],
        ["#FEF3C7", "#FACC15", "#92400E"],
        ["#FEF9C3", "#FBBF24", "#78350F"],
        ["#FCD34D", "#F59E0B", "#92400E"],
        ["#FDE047", "#EAB308", "#78350F"],
        ["#FEF08A", "#FACC15", "#A16207"],
        ["#FEF3C7", "#FCD34D", "#B45309"],
        ["#FDE68A", "#F59E0B", "#92400E"],
        ["#FEF9C3", "#FACC15", "#B45309"],
        ["#FCD34D", "#FBBF24", "#78350F"],
        ["#FDE047", "#F59E0B", "#A16207"],
        ["#FEF08A", "#EAB308", "#92400E"],
        ["#FEF3C7", "#FBBF24", "#78350F"],
        ["#FDE68A", "#FACC15", "#B45309"]
      ];
      
      variants.forEach((variant, index) => {
        const colors = gradientColors[index % gradientColors.length];
        
        let barGradient;
        if (variant.quantity <= 0) {
          barGradient = "linear-gradient(90deg, #7f1d1d 0%, #991b1b 100%)";
        } else if (variant.quantity === 1) {
          barGradient = "linear-gradient(90deg, #dc2626 0%, #b91c1c 100%)";
        } else if (variant.quantity === 2) {
          barGradient = "linear-gradient(90deg, #f87171 0%, #ef4444 100%)";
        } else if (variant.quantity >= 3 && variant.quantity <= 5) {
          barGradient = "linear-gradient(90deg, #fb923c 0%, #f97316 100%)";
        } else if (variant.quantity >= 6 && variant.quantity <= 10) {
          barGradient = "linear-gradient(90deg, #fbbf24 0%, #f59e0b 100%)";
        } else {
          barGradient = "linear-gradient(90deg, #10b981 0%, #059669 100%)";
        }
        
        const maxQuantity = variant.maxQuantity || 30;
        const fillRatio = Math.min(1, variant.quantity / maxQuantity);
        
        const variantEl = document.createElement("div");
        variantEl.className = "flex flex-col gap-1 flex-shrink-0";
        variantEl.innerHTML = `
          <div class="flex items-center justify-between gap-1">
            <div class="relative w-6 h-6 flex-shrink-0">
              <svg viewBox="0 0 64 64" class="w-full h-full drop-shadow-md">
                <defs>
                  <linearGradient id="jar${variant.id}Gradient" x1="0" y1="0" x2="0" y2="1">
                    <stop offset="0%" stop-color="${colors[0]}" />
                    <stop offset="45%" stop-color="${colors[1]}" />
                    <stop offset="100%" stop-color="${colors[2]}" />
                  </linearGradient>
                </defs>
                <rect x="14" y="18" width="36" height="32" rx="10" fill="url(#jar${variant.id}Gradient)" />
                <rect x="18" y="12" width="28" height="7" rx="3.5" fill="#78350F" />
              </svg>
            </div>
            <p class="text-xs font-bold flex-1 min-w-0" style="color: #f0fdfa;">
              ${variant.name}
            </p>
            <span class="text-sm font-bold flex-shrink-0" style="color: rgba(240, 253, 250, 0.95);">
              ${formatNumber(Math.round(variant.quantity))}
            </span>
          </div>
          <div class="w-full h-1.5 rounded-full overflow-hidden shadow-inner" style="background: rgba(3, 45, 39, 0.6);">
            <div class="h-full rounded-full shadow-sm" style="width: ${fillRatio * 100}%; transition: width 0.6s ease-out, background 0.6s ease-out; background: ${barGradient};"></div>
          </div>
        `;
        refs.variantsList.appendChild(variantEl);
      });

      currentQty = total;
      updateMainDisplay();
      checkInventoryAlerts();
    }

    function updateMainDisplay() {
      refs.remainingValue.textContent = formatNumber(Math.max(0, Math.round(currentQty)));

      const ratio = openingQty === 0 ? 0 : Math.max(0, Math.min(1, currentQty / openingQty));
      const pct = Math.round(ratio * 100);
      refs.progressBar.style.width = pct + "%";

      let barGradient;
      let statusText;
      let statusClass;
      let arrowIcon;
      let arrowClass;
      
      if (pct === 0) {
        barGradient = "linear-gradient(90deg, #450a0a 0%, #7f1d1d 100%)";
        statusText = "نفدت الكمية";
        statusClass = "px-1.5 py-0.5 rounded-full text-[8px] font-semibold";
        statusClass += " bg-red-950/40 text-red-200 border border-red-900/90";
        arrowIcon = "⬇";
        arrowClass = "text-[9px] text-red-200";
      } else if (pct >= 1 && pct <= 6) {
        barGradient = "linear-gradient(90deg, #7f1d1d 0%, #991b1b 100%)";
        statusText = "حرج جداً";
        statusClass = "px-1.5 py-0.5 rounded-full text-[8px] font-semibold";
        statusClass += " bg-red-900/35 text-red-100 border border-red-800/80";
        arrowIcon = "⬇";
        arrowClass = "text-[9px] text-red-200";
      } else if (pct >= 7 && pct <= 13) {
        barGradient = "linear-gradient(90deg, #991b1b 0%, #b91c1c 100%)";
        statusText = "حرج";
        statusClass = "px-1.5 py-0.5 rounded-full text-[8px] font-semibold";
        statusClass += " bg-red-800/30 text-red-50 border border-red-700/70";
        arrowIcon = "⬇";
        arrowClass = "text-[9px] text-red-300";
      } else if (pct >= 14 && pct <= 19) {
        barGradient = "linear-gradient(90deg, #dc2626 0%, #ef4444 100%)";
        statusText = "خطر";
        statusClass = "px-1.5 py-0.5 rounded-full text-[8px] font-semibold";
        statusClass += " bg-red-600/25 text-red-50 border border-red-500/60";
        arrowIcon = "⬇";
        arrowClass = "text-[9px] text-red-300";
      } else if (pct >= 20 && pct <= 32) {
        barGradient = "linear-gradient(90deg, #ea580c 0%, #f97316 100%)";
        statusText = "تحذير";
        statusClass = "px-1.5 py-0.5 rounded-full text-[8px] font-semibold";
        statusClass += " bg-orange-600/25 text-orange-50 border border-orange-500/60";
        arrowIcon = "▼";
        arrowClass = "text-[9px] text-orange-300";
      } else if (pct >= 33 && pct <= 64) {
        barGradient = "linear-gradient(90deg, #f59e0b 0%, #fbbf24 100%)";
        statusText = "مخزون جيد";
        statusClass = "px-1.5 py-0.5 rounded-full text-[8px] font-semibold";
        statusClass += " bg-amber-500/25 text-amber-50 border border-amber-400/70";
        arrowIcon = "◆";
        arrowClass = "text-[9px] text-amber-200";
      } else {
        barGradient = "linear-gradient(90deg, #059669 0%, #10b981 100%)";
        statusText = "مخزون ممتاز";
        statusClass = "px-1.5 py-0.5 rounded-full text-[8px] font-semibold";
        statusClass += " bg-emerald-500/25 text-emerald-100 border border-emerald-400/60";
        arrowIcon = "⬆";
        arrowClass = "text-[9px] text-emerald-300";
      }
      
      refs.progressBar.style.background = barGradient;
      refs.statusLabel.textContent = statusText;
      refs.statusLabel.className = statusClass;
      refs.changePercent.textContent = pct + "%";
      refs.changeArrow.textContent = arrowIcon;
      refs.changeArrow.className = arrowClass;
    }

    updateVariantsDisplay();

    // Login System
    const loginScreen = document.getElementById("login-screen");
    const appScreen = document.getElementById("app-screen");
    const loginForm = document.getElementById("login-form");
    const loginError = document.getElementById("login-error");
    const usernameInput = document.getElementById("username");
    const passwordInput = document.getElementById("password");
    const logoutBtn = document.getElementById("logout-btn");
    const userNameDisplay = document.getElementById("user-name-display");
    const userRoleBadge = document.getElementById("user-role-badge");
    const toggleControlPanelBtn = document.getElementById("toggle-control-panel");

    loginForm.addEventListener("submit", (e) => {
      e.preventDefault();
      
      const username = usernameInput.value.trim();
      const password = passwordInput.value.trim();
      
      if (users[username] && users[username].password === password) {
        currentUser = {
          username: username,
          role: users[username].role
        };
        
        loginError.classList.add("hidden");
        loginScreen.classList.add("hidden");
        appScreen.classList.remove("hidden");
        
        userNameDisplay.textContent = username;
        
        if (currentUser.role === "admin") {
          userRoleBadge.textContent = "مدير";
          userRoleBadge.style.cssText = "background: rgba(94, 234, 212, 0.2); color: #5eead4;";
          toggleControlPanelBtn.classList.remove("hidden");
          
          document.getElementById("mode-screen-share").style.display = "inline-block";
          document.getElementById("mode-videos").style.display = "inline-block";
          document.getElementById("mode-images").style.display = "inline-block";
          document.getElementById("screen-share-controls").style.display = "flex";
          document.getElementById("videos-controls").style.display = "none";
          document.getElementById("images-controls").style.display = "none";
          document.getElementById("gallery-upload-btn").style.display = "inline-block";
          document.getElementById("gallery-title").style.display = "inline-block";
        } else {
          userRoleBadge.textContent = "مشاهد";
          userRoleBadge.style.cssText = "background: rgba(251, 191, 36, 0.2); color: #fbbf24;";
          toggleControlPanelBtn.classList.add("hidden");
          
          document.getElementById("mode-screen-share").style.display = "none";
          document.getElementById("mode-videos").style.display = "none";
          document.getElementById("mode-images").style.display = "none";
          document.getElementById("screen-share-controls").style.display = "none";
          document.getElementById("videos-controls").style.display = "none";
          document.getElementById("images-controls").style.display = "none";
          document.getElementById("gallery-upload-btn").style.display = "none";
          document.getElementById("gallery-title").style.display = "none";
        }
        
        usernameInput.value = "";
        passwordInput.value = "";
        
        updateUIForUserRole();
      } else {
        loginError.classList.remove("hidden");
      }
    });

    logoutBtn.addEventListener("click", () => {
      currentUser = null;
      appScreen.classList.add("hidden");
      loginScreen.classList.remove("hidden");
      usernameInput.value = "";
      passwordInput.value = "";
      loginError.classList.add("hidden");
    });

    function updateUIForUserRole() {
      if (currentUser && currentUser.role === "viewer") {
        if (imageInfoOverlay) {
          imageInfoOverlay.style.display = "none";
        }
      } else {
        if (imageInfoOverlay) {
          imageInfoOverlay.style.display = "";
        }
      }
    }

    // Video System
    let uploadedVideos = [];
    let currentVideoIndex = 0;
    let isVideoPlaying = false;
    let currentVideoElement = null;
    const videoUploadInput = document.getElementById("video-upload-input");
    const videoPlayerContainer = document.getElementById("video-player-container");
    const videoProgressBar = document.getElementById("video-progress-bar");
    const videoProgressFill = document.getElementById("video-progress-fill");
    const videoInfoOverlay = document.getElementById("video-info-overlay");
    const videoCurrentName = document.getElementById("video-current-name");
    const videoCounter = document.getElementById("video-counter");
    const toggleVideoPlaybackBtn = document.getElementById("toggle-video-playback");

    videoUploadInput.addEventListener("change", (e) => {
      const files = Array.from(e.target.files);
      
      if (files.length === 0) return;
      
      let processedCount = 0;
      const totalFiles = files.length;
      
      files.forEach(file => {
        if (file.type.startsWith("video/")) {
          const reader = new FileReader();
          
          reader.onload = (event) => {
            uploadedVideos.push({
              id: Date.now() + Math.random(),
              src: event.target.result,
              name: file.name
            });
            
            processedCount++;
            
            if (processedCount === totalFiles) {
              if (uploadedVideos.length === 1) {
                loadVideo(0);
              }
              updateVideoCounter();
            }
          };
          
          reader.onerror = () => {
            processedCount++;
          };
          
          reader.readAsDataURL(file);
        } else {
          processedCount++;
        }
      });
      
      e.target.value = "";
    });

    function loadVideo(index) {
      if (uploadedVideos.length === 0) return;
      
      currentVideoIndex = index;
      const video = uploadedVideos[currentVideoIndex];
      
      if (currentVideoElement) {
        currentVideoElement.classList.add("video-fade-out");
        setTimeout(() => {
          if (currentVideoElement && currentVideoElement.parentNode) {
            currentVideoElement.remove();
          }
          createVideoElement(video);
        }, 800);
      } else {
        createVideoElement(video);
      }
    }

    function createVideoElement(video) {
      videoPlayerContainer.innerHTML = `
        <div class="w-full h-full flex items-center justify-center">
          <div class="text-center">
            <p class="text-2xl mb-2">⏳</p>
            <p class="text-sm font-bold" style="color: #5eead4;">جاري تحميل الفيديو...</p>
            <p class="text-xs mt-2" style="color: rgba(240, 253, 250, 0.7);">قد يستغرق بضع ثوان</p>
          </div>
        </div>
      `;
      
      const videoEl = document.createElement("video");
      videoEl.className = "video-fade-in";
      videoEl.style.cssText = "position: absolute; top: 0; left: 0; width: 100%; height: 100%; object-fit: contain; background: #000;";
      videoEl.muted = false;
      videoEl.playsInline = true;
      videoEl.controls = false;
      videoEl.preload = "auto";
      
      currentVideoElement = videoEl;
      
      videoCurrentName.textContent = video.name;
      updateVideoCounter();
      
      let isLoaded = false;
      let loadTimeout = null;
      let hasShownError = false;
      
      const showVideoPlayer = () => {
        if (isLoaded || hasShownError) return;
        isLoaded = true;
        
        if (loadTimeout) {
          clearTimeout(loadTimeout);
          loadTimeout = null;
        }
        
        videoPlayerContainer.innerHTML = "";
        videoPlayerContainer.appendChild(videoEl);
        
        if (isVideoPlaying) {
          setTimeout(() => {
            const playPromise = videoEl.play();
            if (playPromise !== undefined) {
              playPromise.then(() => {
                videoProgressBar.classList.remove("hidden");
                videoInfoOverlay.classList.remove("hidden");
              }).catch(err => {
                console.log("Video play error:", err);
                if (!hasShownError) {
                  videoPlayerContainer.innerHTML = `
                    <div class="text-center px-4">
                      <p class="text-lg mb-2">▶️</p>
                      <p class="text-sm font-bold mb-2" style="color: #5eead4;">الفيديو جاهز للتشغيل</p>
                      <p class="text-xs" style="color: rgba(240, 253, 250, 0.7);">
                        اضغط زر التشغيل لبدء العرض
                      </p>
                    </div>
                  `;
                }
              });
            }
          }, 300);
        }
      };
      
      videoEl.addEventListener("loadedmetadata", () => {
        if (videoEl.duration && videoEl.duration > 0) {
          showVideoPlayer();
        }
      });
      
      videoEl.addEventListener("loadeddata", () => {
        showVideoPlayer();
      });
      
      videoEl.addEventListener("canplay", () => {
        showVideoPlayer();
      });
      
      videoEl.addEventListener("canplaythrough", () => {
        showVideoPlayer();
      });
      
      videoEl.addEventListener("timeupdate", () => {
        if (videoEl.duration && !isNaN(videoEl.duration) && videoEl.duration > 0) {
          const progress = (videoEl.currentTime / videoEl.duration) * 100;
          videoProgressFill.style.width = progress + "%";
        }
      });
      
      videoEl.addEventListener("ended", () => {
        playNextVideo();
      });
      
      videoEl.addEventListener("error", (e) => {
        if (hasShownError) return;
        hasShownError = true;
        
        if (loadTimeout) {
          clearTimeout(loadTimeout);
          loadTimeout = null;
        }
        
        let errorMsg = "🎬 صيغة الفيديو غير مدعومة";
        let errorDetails = "يرجى استخدام فيديو بصيغة MP4 (H.264) للتوافق الأمثل";
        
        videoPlayerContainer.innerHTML = `
          <div class="text-center px-6">
            <p class="text-4xl mb-4">🎬</p>
            <p class="text-lg font-bold mb-3" style="color: #fbbf24;">${errorMsg}</p>
            <p class="text-sm mb-4 leading-relaxed" style="color: rgba(240, 253, 250, 0.9);">
              ${errorDetails}
            </p>
            <button
              onclick="document.getElementById('video-upload-input').click()"
              class="px-5 py-3 rounded-lg font-bold text-sm transition-all duration-300 hover:scale-105"
              style="background: linear-gradient(135deg, #14b8a6 0%, #0d9488 100%); color: #f0fdfa;"
            >
              📤 رفع فيديو آخر
            </button>
          </div>
        `;
      });
      
      loadTimeout = setTimeout(() => {
        if (!isLoaded && !hasShownError) {
          videoPlayerContainer.innerHTML = `
            <div class="text-center px-6">
              <p class="text-2xl mb-3">⏱️</p>
              <p class="text-sm font-bold mb-2" style="color: #fbbf24;">التحميل يستغرق وقتاً طويلاً</p>
              <p class="text-xs mb-3 leading-relaxed" style="color: rgba(240, 253, 250, 0.8);">
                الفيديو كبير الحجم أو الاتصال بطيء.<br/>
                يرجى الانتظار أو تجربة فيديو أصغر.
              </p>
            </div>
          `;
        }
      }, 8000);
      
      try {
        videoEl.src = video.src;
        videoEl.load();
      } catch (err) {
        console.error("Error loading video:", err);
        if (!hasShownError) {
          hasShownError = true;
          videoPlayerContainer.innerHTML = `
            <div class="text-center px-6">
              <p class="text-3xl mb-3">❌</p>
              <p class="text-base font-bold mb-2" style="color: #fbbf24;">فشل تحميل الفيديو</p>
              <p class="text-sm leading-relaxed" style="color: rgba(240, 253, 250, 0.8);">
                حدث خطأ غير متوقع. جرب فيديو آخر.
              </p>
            </div>
          `;
        }
      }
    }

    function playNextVideo() {
      if (uploadedVideos.length === 0) return;
      
      currentVideoIndex = (currentVideoIndex + 1) % uploadedVideos.length;
      loadVideo(currentVideoIndex);
    }

    function updateVideoCounter() {
      if (uploadedVideos.length > 0) {
        videoCounter.textContent = `${currentVideoIndex + 1} / ${uploadedVideos.length}`;
      }
    }

    toggleVideoPlaybackBtn.addEventListener("click", () => {
      if (uploadedVideos.length === 0) return;
      
      if (!isVideoPlaying) {
        isVideoPlaying = true;
        toggleVideoPlaybackBtn.innerHTML = "⏸️ إيقاف";
        toggleVideoPlaybackBtn.style.background = "linear-gradient(135deg, #ef4444 0%, #dc2626 100%)";
        
        if (currentVideoElement) {
          currentVideoElement.play().catch(err => console.log("Video play error:", err));
          videoProgressBar.classList.remove("hidden");
          videoInfoOverlay.classList.remove("hidden");
        } else {
          loadVideo(0);
        }
      } else {
        isVideoPlaying = false;
        toggleVideoPlaybackBtn.innerHTML = "▶️ تشغيل";
        toggleVideoPlaybackBtn.style.background = "linear-gradient(135deg, #10b981 0%, #059669 100%)";
        
        if (currentVideoElement) {
          currentVideoElement.pause();
        }
      }
    });

    // Mode Switching System
    const modeScreenShareBtn = document.getElementById("mode-screen-share");
    const modeVideosBtn = document.getElementById("mode-videos");
    const modeImagesBtn = document.getElementById("mode-images");
    const screenShareControls = document.getElementById("screen-share-controls");
    const videosControls = document.getElementById("videos-controls");
    const imagesControls = document.getElementById("images-controls");
    const screenShareDisplay = document.getElementById("screen-share-display");
    const videosDisplay = document.getElementById("videos-display");
    const imagesDisplay = document.getElementById("images-display");

    modeScreenShareBtn.addEventListener("click", () => {
      modeScreenShareBtn.style.cssText = "background: linear-gradient(135deg, #14b8a6 0%, #0d9488 100%); color: #f0fdfa;";
      modeVideosBtn.style.cssText = "background: rgba(94, 234, 212, 0.2); color: rgba(240, 253, 250, 0.7);";
      modeImagesBtn.style.cssText = "background: rgba(94, 234, 212, 0.2); color: rgba(240, 253, 250, 0.7);";
      
      screenShareControls.style.display = "flex";
      videosControls.style.display = "none";
      imagesControls.style.display = "none";
      
      screenShareDisplay.classList.remove("hidden");
      videosDisplay.classList.add("hidden");
      imagesDisplay.classList.add("hidden");
      
      if (isVideoPlaying && currentVideoElement) {
        currentVideoElement.pause();
      }
      if (isImageSlideshowPlaying) {
        isImageSlideshowPlaying = false;
        toggleImageSlideshowBtn.innerHTML = "▶️ تشغيل";
        toggleImageSlideshowBtn.style.background = "linear-gradient(135deg, #10b981 0%, #059669 100%)";
      }
    });

    modeVideosBtn.addEventListener("click", () => {
      modeVideosBtn.style.cssText = "background: linear-gradient(135deg, #14b8a6 0%, #0d9488 100%); color: #f0fdfa;";
      modeScreenShareBtn.style.cssText = "background: rgba(94, 234, 212, 0.2); color: rgba(240, 253, 250, 0.7);";
      modeImagesBtn.style.cssText = "background: rgba(94, 234, 212, 0.2); color: rgba(240, 253, 250, 0.7);";
      
      videosControls.style.display = "flex";
      screenShareControls.style.display = "none";
      imagesControls.style.display = "none";
      
      videosDisplay.classList.remove("hidden");
      screenShareDisplay.classList.add("hidden");
      imagesDisplay.classList.add("hidden");
      
      if (isScreenSharing) {
        stopScreenShare();
      }
      if (isImageSlideshowPlaying) {
        isImageSlideshowPlaying = false;
        toggleImageSlideshowBtn.innerHTML = "▶️ تشغيل";
        toggleImageSlideshowBtn.style.background = "linear-gradient(135deg, #10b981 0%, #059669 100%)";
      }
    });

    modeImagesBtn.addEventListener("click", () => {
      modeImagesBtn.style.cssText = "background: linear-gradient(135deg, #14b8a6 0%, #0d9488 100%); color: #f0fdfa;";
      modeScreenShareBtn.style.cssText = "background: rgba(94, 234, 212, 0.2); color: rgba(240, 253, 250, 0.7);";
      modeVideosBtn.style.cssText = "background: rgba(94, 234, 212, 0.2); color: rgba(240, 253, 250, 0.7);";
      
      imagesControls.style.display = "flex";
      screenShareControls.style.display = "none";
      videosControls.style.display = "none";
      
      imagesDisplay.classList.remove("hidden");
      screenShareDisplay.classList.add("hidden");
      videosDisplay.classList.add("hidden");
      
      if (isScreenSharing) {
        stopScreenShare();
      }
      if (isVideoPlaying && currentVideoElement) {
        currentVideoElement.pause();
      }
    });

    // Image Slideshow System
    let displayImages = [];
    let currentImageIndex = 0;
    let isImageSlideshowPlaying = false;
    let imageSlideshowInterval = null;
    const displayImageUploadInput = document.getElementById("display-image-upload-input");
    const imageSlideshowContainer = document.getElementById("image-slideshow-container");
    const imageInfoOverlay = document.getElementById("image-info-overlay");
    const imageCurrentName = document.getElementById("image-current-name");
    const imageCounter = document.getElementById("image-counter");
    const toggleImageSlideshowBtn = document.getElementById("toggle-image-slideshow");

    displayImageUploadInput.addEventListener("change", (e) => {
      const files = Array.from(e.target.files);
      
      if (files.length === 0) return;
      
      let processedCount = 0;
      const totalFiles = files.length;
      
      files.forEach(file => {
        if (file.type.startsWith("image/")) {
          const reader = new FileReader();
          
          reader.onload = (event) => {
            displayImages.push({
              id: Date.now() + Math.random(),
              src: event.target.result,
              name: file.name
            });
            
            processedCount++;
            
            if (processedCount === totalFiles) {
              if (displayImages.length === 1) {
                showImage(0);
              }
              updateImageCounter();
            }
          };
          
          reader.onerror = () => {
            processedCount++;
          };
          
          reader.readAsDataURL(file);
        } else {
          processedCount++;
        }
      });
      
      e.target.value = "";
    });

    function showImage(index) {
      if (displayImages.length === 0) return;
      
      currentImageIndex = index;
      const image = displayImages[currentImageIndex];
      
      imageSlideshowContainer.innerHTML = `
        <img src="${image.src}" alt="${image.name}" class="video-fade-in" style="position: absolute; top: 0; left: 0; width: 100%; height: 100%; object-fit: contain;" />
      `;
      
      imageCurrentName.textContent = image.name;
      updateImageCounter();
      
      if (currentUser && currentUser.role === "admin") {
        imageInfoOverlay.classList.remove("hidden");
      } else {
        imageInfoOverlay.classList.add("hidden");
      }
    }

    function showNextImage() {
      if (displayImages.length === 0) return;
      
      currentImageIndex = (currentImageIndex + 1) % displayImages.length;
      showImage(currentImageIndex);
    }

    function updateImageCounter() {
      if (displayImages.length > 0) {
        imageCounter.textContent = `${currentImageIndex + 1} / ${displayImages.length}`;
      }
    }

    toggleImageSlideshowBtn.addEventListener("click", () => {
      if (displayImages.length === 0) return;
      
      if (!isImageSlideshowPlaying) {
        isImageSlideshowPlaying = true;
        toggleImageSlideshowBtn.innerHTML = "⏸️ إيقاف";
        toggleImageSlideshowBtn.style.background = "linear-gradient(135deg, #ef4444 0%, #dc2626 100%)";
        
        showImage(currentImageIndex);
        
        imageSlideshowInterval = setInterval(() => {
          showNextImage();
        }, 10000);
      } else {
        isImageSlideshowPlaying = false;
        toggleImageSlideshowBtn.innerHTML = "▶️ تشغيل";
        toggleImageSlideshowBtn.style.background = "linear-gradient(135deg, #10b981 0%, #059669 100%)";
        
        if (imageSlideshowInterval) {
          clearInterval(imageSlideshowInterval);
          imageSlideshowInterval = null;
        }
      }
    });

    // Screen Share System
    let isScreenSharing = false;
    let screenShareStream = null;
    const toggleScreenShareBtn = document.getElementById("toggle-screen-share");

    toggleScreenShareBtn.addEventListener("click", async () => {
      if (!isScreenSharing) {
        if (!navigator.mediaDevices || !navigator.mediaDevices.getDisplayMedia) {
          screenShareDisplay.innerHTML = `
            <div class="text-center px-6">
              <p class="text-lg mb-3">🚫</p>
              <p class="text-sm font-bold mb-2" style="color: #f0fdfa;">مشاركة الشاشة غير متاحة</p>
              <p class="text-xs leading-relaxed" style="color: rgba(240, 253, 250, 0.7);">
                متصفحك أو البيئة الحالية لا تدعم مشاركة الشاشة.<br/>
                جرب فتح الصفحة في نافذة منفصلة أو استخدم متصفح حديث.
              </p>
            </div>
          `;
          return;
        }

        try {
          screenShareDisplay.innerHTML = `
            <div class="text-center px-6">
              <p class="text-2xl mb-3">🖥️</p>
              <p class="text-sm font-bold" style="color: #f0fdfa;">جاري طلب مشاركة الشاشة...</p>
              <p class="text-xs mt-2" style="color: rgba(240, 253, 250, 0.7);">اختر نافذة أو تبويب أو شاشة كاملة</p>
            </div>
          `;

          screenShareStream = await navigator.mediaDevices.getDisplayMedia({ 
            video: {
              cursor: "always",
              displaySurface: "browser"
            },
            audio: false
          });
          
          const videoElement = document.createElement("video");
          videoElement.srcObject = screenShareStream;
          videoElement.autoplay = true;
          videoElement.muted = true;
          videoElement.playsInline = true;
          videoElement.style.cssText = "position: absolute; top: 0; left: 0; width: 100%; height: 100%; object-fit: contain; background: #000;";
          
          videoElement.onloadedmetadata = () => {
            screenShareDisplay.innerHTML = "";
            screenShareDisplay.appendChild(videoElement);
            videoElement.play().catch(e => console.log("Play error:", e));
          };
          
          toggleScreenShareBtn.textContent = "إيقاف المشاركة";
          toggleScreenShareBtn.style.background = "linear-gradient(135deg, #ef4444 0%, #dc2626 100%)";
          isScreenSharing = true;
          
          screenShareStream.getVideoTracks()[0].onended = () => {
            stopScreenShare();
          };
        } catch (err) {
          console.error("Screen share error:", err);
          
          let errorMessage = "لم يتم منح إذن مشاركة الشاشة";
          
          if (err.name === "NotAllowedError") {
            errorMessage = "تم رفض إذن مشاركة الشاشة. يرجى السماح بالوصول والمحاولة مرة أخرى.";
          } else if (err.name === "NotSupportedError") {
            errorMessage = "مشاركة الشاشة غير مدعومة في هذا المتصفح أو البيئة.";
          } else if (err.name === "NotFoundError") {
            errorMessage = "لم يتم العثور على شاشة للمشاركة.";
          } else if (err.name === "AbortError") {
            errorMessage = "تم إلغاء مشاركة الشاشة.";
          }
          
          screenShareDisplay.innerHTML = `
            <div class="text-center px-6">
              <p class="text-2xl mb-3">⚠️</p>
              <p class="text-base font-bold mb-2" style="color: #fbbf24;">خطأ في مشاركة الشاشة</p>
              <p class="text-sm leading-relaxed" style="color: rgba(240, 253, 250, 0.8);">
                ${errorMessage}
              </p>
            </div>
          `;
        }
      } else {
        stopScreenShare();
      }
    });

    function stopScreenShare() {
      if (screenShareStream) {
        screenShareStream.getTracks().forEach(track => track.stop());
        screenShareStream = null;
      }
      screenShareDisplay.innerHTML = `<p class="text-base" style="color: rgba(240, 253, 250, 0.6);">انقر "بدء المشاركة" لعرض الشاشة</p>`;
      toggleScreenShareBtn.textContent = "بدء المشاركة";
      toggleScreenShareBtn.style.background = "linear-gradient(135deg, #14b8a6 0%, #0d9488 100%)";
      isScreenSharing = false;
    }

    // Image Upload System (Gallery)
    const imageUploadInput = document.getElementById("image-upload-input");
    const imagesGallery = document.getElementById("images-gallery");
    let uploadedImages = [];
    let currentGalleryIndex = 0;
    let galleryInterval = null;
    let isGalleryTransitioning = false;

    imageUploadInput.addEventListener("change", (e) => {
      const files = Array.from(e.target.files);
      
      if (files.length === 0) return;
      
      let processedCount = 0;
      const totalFiles = files.length;
      
      files.forEach(file => {
        if (file.type.startsWith("image/")) {
          const reader = new FileReader();
          
          reader.onload = (event) => {
            uploadedImages.push({
              id: Date.now() + Math.random(),
              src: event.target.result,
              name: file.name
            });
            
            processedCount++;
            
            if (processedCount === totalFiles) {
              if (uploadedImages.length === files.length) {
                currentGalleryIndex = 0;
              }
              renderImagesGallery(false);
              startGallerySlideshow();
            }
          };
          
          reader.onerror = () => {
            processedCount++;
          };
          
          reader.readAsDataURL(file);
        } else {
          processedCount++;
        }
      });
      
      e.target.value = "";
    });

    function renderImagesGallery(withAnimation = false) {
      if (uploadedImages.length === 0) {
        imagesGallery.innerHTML = "";
        const placeholderCard = document.createElement("div");
        placeholderCard.className = "rounded-lg flex items-center justify-center h-full";
        placeholderCard.style.cssText = "background: rgba(3, 45, 39, 0.6); border: 2px dashed rgba(94, 234, 212, 0.3);";
        placeholderCard.innerHTML = `
          <p class="text-[10px] text-center px-2" style="color: rgba(240, 253, 250, 0.6);">
            ارفع صور المنتجات
          </p>
        `;
        imagesGallery.appendChild(placeholderCard);
        return;
      }
      
      const currentImage = uploadedImages[currentGalleryIndex];
      
      if (withAnimation && !isGalleryTransitioning) {
        isGalleryTransitioning = true;
        
        const existingCard = imagesGallery.querySelector(".relative");
        if (existingCard) {
          existingCard.classList.add("gallery-slide-out-left");
          
          setTimeout(() => {
            imagesGallery.innerHTML = "";
            createGalleryCard(currentImage, "gallery-slide-in-right");
            isGalleryTransitioning = false;
          }, 800);
        } else {
          imagesGallery.innerHTML = "";
          createGalleryCard(currentImage, "gallery-slide-in-right");
          isGalleryTransitioning = false;
        }
      } else {
        imagesGallery.innerHTML = "";
        createGalleryCard(currentImage, "gallery-zoom-pulse");
      }
    }

    function createGalleryCard(currentImage, animationClass) {
      const imageCard = document.createElement("div");
      imageCard.className = `relative rounded-lg overflow-hidden h-full group ${animationClass}`;
      imageCard.style.cssText = "background: rgba(3, 45, 39, 0.6); border: 2px solid rgba(94, 234, 212, 0.3);";
      
      imageCard.innerHTML = `
        <img src="${currentImage.src}" alt="${currentImage.name}" style="position: absolute; top: 0; left: 0; width: 100%; height: 100%; object-fit: cover;" />
        <div class="absolute bottom-0 left-0 right-0 px-2 py-1.5 backdrop-blur-sm" style="background: rgba(5, 66, 57, 0.9); border-top: 1px solid rgba(94, 234, 212, 0.4);">
          <div class="flex items-center justify-between">
            <span class="text-[9px] font-bold truncate flex-1" style="color: #f0fdfa;">${currentImage.name}</span>
            <span class="text-[8px] ml-2" style="color: rgba(240, 253, 250, 0.7);">${currentGalleryIndex + 1} / ${uploadedImages.length}</span>
          </div>
        </div>
        <button
          class="delete-gallery-image-btn absolute top-1 right-1 w-6 h-6 rounded-full flex items-center justify-center opacity-0 group-hover:opacity-100 transition-opacity duration-300"
          style="background: rgba(220, 38, 38, 0.9); color: #fef2f2;"
          title="حذف الصورة"
        >
          ✕
        </button>
        <div class="absolute top-1 left-1 flex gap-1">
          <button
            class="prev-gallery-image-btn w-6 h-6 rounded-full flex items-center justify-center opacity-0 group-hover:opacity-100 transition-opacity duration-300"
            style="background: rgba(20, 184, 166, 0.9); color: #f0fdfa;"
            title="السابق"
          >
            ◀
          </button>
          <button
            class="next-gallery-image-btn w-6 h-6 rounded-full flex items-center justify-center opacity-0 group-hover:opacity-100 transition-opacity duration-300"
            style="background: rgba(20, 184, 166, 0.9); color: #f0fdfa;"
            title="التالي"
          >
            ▶
          </button>
        </div>
      `;
      
      imagesGallery.appendChild(imageCard);
      
      imageCard.querySelector(".delete-gallery-image-btn").addEventListener("click", () => {
        uploadedImages.splice(currentGalleryIndex, 1);
        if (currentGalleryIndex >= uploadedImages.length && currentGalleryIndex > 0) {
          currentGalleryIndex = uploadedImages.length - 1;
        }
        renderImagesGallery(false);
        if (uploadedImages.length === 0) {
          stopGallerySlideshow();
        }
      });
      
      imageCard.querySelector(".prev-gallery-image-btn").addEventListener("click", () => {
        currentGalleryIndex = (currentGalleryIndex - 1 + uploadedImages.length) % uploadedImages.length;
        renderImagesGallery(true);
      });
      
      imageCard.querySelector(".next-gallery-image-btn").addEventListener("click", () => {
        currentGalleryIndex = (currentGalleryIndex + 1) % uploadedImages.length;
        renderImagesGallery(true);
      });
    }

    function startGallerySlideshow() {
      if (galleryInterval || uploadedImages.length === 0) return;
      
      galleryInterval = setInterval(() => {
        if (uploadedImages.length > 0) {
          currentGalleryIndex = (currentGalleryIndex + 1) % uploadedImages.length;
          renderImagesGallery(true);
        }
      }, 60000);
    }

    function stopGallerySlideshow() {
      if (galleryInterval) {
        clearInterval(galleryInterval);
        galleryInterval = null;
      }
    }

    // Comments System
    const commentsContainer = document.getElementById("comments-container");
    let commentIndex = 0;

    function showComment() {
      if (defaultComments.length === 0) return;
      
      const comment = defaultComments[commentIndex];
      commentIndex = (commentIndex + 1) % defaultComments.length;

      const commentEl = document.createElement("div");
      commentEl.className = "comment-slide-in rounded-lg border px-3 py-2.5 shadow-xl backdrop-blur-sm";
      commentEl.style.cssText = "background: linear-gradient(135deg, #428177 0%, #357066 50%, #2a5a52 100%); border-color: rgba(94, 234, 212, 0.4);";
      commentEl.innerHTML = `
        <div class="flex items-start gap-2.5">
          <div class="w-9 h-9 rounded-full flex items-center justify-center text-white font-bold text-sm shadow-md flex-shrink-0" style="background: linear-gradient(135deg, #5eead4 0%, #14b8a6 100%);">
            ${comment.name.charAt(0)}
          </div>
          <div class="flex-1 min-w-0">
            <p class="text-xs font-bold mb-1" style="color: #f0fdfa;">
              ${comment.name}
            </p>
            <p class="text-[11px] leading-relaxed" style="color: rgba(240, 253, 250, 0.95);">
              ${comment.text}
            </p>
          </div>
          <div class="text-base flex-shrink-0" style="color: rgba(94, 234, 212, 0.7);">
            ✨
          </div>
        </div>
      `;

      commentsContainer.appendChild(commentEl);

      setTimeout(() => {
        commentEl.classList.remove("comment-slide-in");
        commentEl.classList.add("comment-slide-out");
        setTimeout(() => {
          commentEl.remove();
        }, 500);
      }, 5000);
    }

    function showManualComment(name, text) {
      const commentEl = document.createElement("div");
      commentEl.className = "comment-slide-in rounded-lg border px-3 py-2.5 shadow-xl backdrop-blur-sm";
      commentEl.style.cssText = "background: linear-gradient(135deg, #428177 0%, #357066 50%, #2a5a52 100%); border-color: rgba(94, 234, 212, 0.4);";
      commentEl.innerHTML = `
        <div class="flex items-start gap-2.5">
          <div class="w-9 h-9 rounded-full flex items-center justify-center text-white font-bold text-sm shadow-md flex-shrink-0" style="background: linear-gradient(135deg, #5eead4 0%, #14b8a6 100%);">
            ${name.charAt(0)}
          </div>
          <div class="flex-1 min-w-0">
            <p class="text-xs font-bold mb-1" style="color: #f0fdfa;">
              ${name}
            </p>
            <p class="text-[11px] leading-relaxed" style="color: rgba(240, 253, 250, 0.95);">
              ${text}
            </p>
          </div>
          <div class="text-base flex-shrink-0" style="color: rgba(94, 234, 212, 0.7);">
            ✨
          </div>
        </div>
      `;

      commentsContainer.appendChild(commentEl);

      setTimeout(() => {
        commentEl.classList.remove("comment-slide-in");
        commentEl.classList.add("comment-slide-out");
        setTimeout(() => {
          commentEl.remove();
        }, 500);
      }, 5000);
    }

    setTimeout(() => {
      showComment();
      setInterval(showComment, 8000);
    }, 3000);

    // Hearts System
    const heartsContainer = document.getElementById("hearts-container");
    let heartsIntervalId = null;

    function createHeart() {
      if (!heartsContainer) return;
      
      const heart = document.createElement("div");
      heart.className = "heart-float absolute";
      
      const randomX = Math.random() * 80 - 40;
      const randomDelay = Math.random() * 0.3;
      const randomSize = 20 + Math.random() * 16;
      const randomLeft = Math.random() * 160 + 20;
      
      heart.style.cssText = `
        bottom: 0;
        left: ${randomLeft}px;
        --drift-x: ${randomX}px;
        animation-delay: ${randomDelay}s;
        font-size: ${randomSize}px;
        filter: drop-shadow(0 1px 4px rgba(220, 38, 38, 0.6));
      `;
      
      heart.textContent = "❤️";
      
      heartsContainer.appendChild(heart);
      
      setTimeout(() => {
        if (heart && heart.parentNode) {
          heart.remove();
        }
      }, 4000);
    }

    function startHearts() {
      if (heartsIntervalId) return;
      
      createHeart();
      
      heartsIntervalId = setInterval(() => {
        createHeart();
      }, 800);
    }

    setTimeout(() => {
      startHearts();
    }, 2000);

    // News Ticker System
    function updateNewsTicker() {
      const newsTickerContent = document.getElementById("news-ticker-content");
      newsTickerContent.innerHTML = "";
      
      const doubledNews = [...newsItems, ...newsItems];
      
      doubledNews.forEach(news => {
        const newsSpan = document.createElement("span");
        newsSpan.className = "text-sm font-bold";
        newsSpan.style.color = "#f0fdfa";
        newsSpan.textContent = news.text;
        newsTickerContent.appendChild(newsSpan);
      });
    }

    updateNewsTicker();

    // Control Panel Button Handler
    toggleControlPanelBtn.addEventListener("click", (e) => {
      e.preventDefault();
      e.stopPropagation();
      
      if (!currentUser || currentUser.role !== "admin") {
        return;
      }
      
      controlPanelOverlay.classList.remove("hidden");
      renderVariantsControlList();
      renderNewsControlList();
    });

    closeControlPanelBtn.addEventListener("click", () => {
      controlPanelOverlay.classList.add("hidden");
    });

    // Control Panel Functions
    function renderVariantsControlList() {
      const variantsControlList = document.getElementById("variants-control-list");
      variantsControlList.innerHTML = "";
      
      variants.forEach((variant) => {
        const variantControl = document.createElement("div");
        variantControl.className = "flex items-center gap-3 p-4 rounded-lg";
        variantControl.style.cssText = "background: rgba(3, 45, 39, 0.6); border: 2px solid rgba(94, 234, 212, 0.2);";
        
        variantControl.innerHTML = `
          <div class="flex-1 grid grid-cols-3 gap-3">
            <div class="flex flex-col gap-2">
              <label class="text-xs font-medium" style="color: #f0fdfa;">اسم الصنف</label>
              <input
                type="text"
                value="${variant.name}"
                data-variant-id="${variant.id}"
                data-field="name"
                class="variant-name-input px-3 py-2 rounded-lg font-bold text-sm"
                style="background: rgba(5, 66, 57, 0.8); color: #f0fdfa; border: 2px solid rgba(94, 234, 212, 0.3);"
              />
            </div>
            <div class="flex flex-col gap-2">
              <label class="text-xs font-medium" style="color: #f0fdfa;">الكمية الحالية (كغ)</label>
              <div class="flex items-center gap-2">
                <button
                  class="decrease-variant-btn w-10 h-10 rounded-lg font-bold text-lg transition-all duration-300 hover:scale-110"
                  data-variant-id="${variant.id}"
                  style="background: linear-gradient(135deg, #ef4444 0%, #dc2626 100%); color: #fef2f2;"
                >
                  −
                </button>
                <input
                  type="number"
                  value="${variant.quantity}"
                  min="0"
                  data-variant-id="${variant.id}"
                  data-field="quantity"
                  class="variant-quantity-input flex-1 px-3 py-2 rounded-lg font-bold text-sm text-center"
                  style="background: rgba(5, 66, 57, 0.8); color: #f0fdfa; border: 2px solid rgba(94, 234, 212, 0.3);"
                />
                <button
                  class="increase-variant-btn w-10 h-10 rounded-lg font-bold text-lg transition-all duration-300 hover:scale-110"
                  data-variant-id="${variant.id}"
                  style="background: linear-gradient(135deg, #10b981 0%, #059669 100%); color: #f0fdfa;"
                >
                  +
                </button>
              </div>
            </div>
            <div class="flex flex-col gap-2">
              <label class="text-xs font-medium" style="color: #f0fdfa;">الحد الأقصى (100%)</label>
              <input
                type="number"
                value="${variant.maxQuantity || 10}"
                min="1"
                data-variant-id="${variant.id}"
                data-field="maxQuantity"
                class="variant-max-input px-3 py-2 rounded-lg font-bold text-sm text-center"
                style="background: rgba(5, 66, 57, 0.8); color: #f0fdfa; border: 2px solid rgba(94, 234, 212, 0.3);"
              />
            </div>
          </div>
          <button
            class="delete-variant-btn w-10 h-10 rounded-lg font-bold text-lg transition-all duration-300 hover:scale-110"
            data-variant-id="${variant.id}"
            style="background: linear-gradient(135deg, #dc2626 0%, #b91c1c 100%); color: #fef2f2;"
            title="حذف الصنف"
          >
            🗑️
          </button>
        `;
        
        variantsControlList.appendChild(variantControl);
      });

      document.querySelectorAll(".variant-name-input").forEach(input => {
        input.addEventListener("change", (e) => {
          const variantId = parseInt(e.target.dataset.variantId);
          const variant = variants.find(v => v.id === variantId);
          if (variant) {
            variant.name = e.target.value;
            updateVariantsDisplay();
            if (currentUser && currentUser.role === 'admin') {
              syncToFirebase('variants', variants);
            }
          }
        });
      });

      document.querySelectorAll(".variant-quantity-input").forEach(input => {
        input.addEventListener("change", (e) => {
          const variantId = parseInt(e.target.dataset.variantId);
          const variant = variants.find(v => v.id === variantId);
          if (variant) {
            variant.quantity = Math.max(0, parseFloat(e.target.value) || 0);
            updateVariantsDisplay();
            if (currentUser && currentUser.role === 'admin') {
              syncToFirebase('variants', variants);
            }
          }
        });
      });

      document.querySelectorAll(".variant-max-input").forEach(input => {
        input.addEventListener("change", (e) => {
          const variantId = parseInt(e.target.dataset.variantId);
          const variant = variants.find(v => v.id === variantId);
          if (variant) {
            variant.maxQuantity = Math.max(1, parseFloat(e.target.value) || 10);
            updateVariantsDisplay();
            if (currentUser && currentUser.role === 'admin') {
              syncToFirebase('variants', variants);
            }
          }
        });
      });

      document.querySelectorAll(".increase-variant-btn").forEach(btn => {
        btn.addEventListener("click", (e) => {
          const variantId = parseInt(e.target.dataset.variantId);
          const variant = variants.find(v => v.id === variantId);
          if (variant) {
            variant.quantity += 1;
            updateVariantsDisplay();
            renderVariantsControlList();
            if (currentUser && currentUser.role === 'admin') {
              syncToFirebase('variants', variants);
            }
          }
        });
      });

      document.querySelectorAll(".decrease-variant-btn").forEach(btn => {
        btn.addEventListener("click", (e) => {
          const variantId = parseInt(e.target.dataset.variantId);
          const variant = variants.find(v => v.id === variantId);
          if (variant) {
            variant.quantity = Math.max(0, variant.quantity - 1);
            updateVariantsDisplay();
            renderVariantsControlList();
            if (currentUser && currentUser.role === 'admin') {
              syncToFirebase('variants', variants);
            }
          }
        });
      });

      document.querySelectorAll(".delete-variant-btn").forEach(btn => {
        btn.addEventListener("click", (e) => {
          const variantId = parseInt(e.target.dataset.variantId);
          variants = variants.filter(v => v.id !== variantId);
          updateVariantsDisplay();
          renderVariantsControlList();
          if (currentUser && currentUser.role === 'admin') {
            syncToFirebase('variants', variants);
          }
        });
      });
    }

    function renderNewsControlList() {
      const newsControlList = document.getElementById("news-control-list");
      newsControlList.innerHTML = "";
      
      newsItems.forEach((news) => {
        const newsControl = document.createElement("div");
        newsControl.className = "flex items-center gap-3 p-4 rounded-lg";
        newsControl.style.cssText = "background: rgba(3, 45, 39, 0.6); border: 2px solid rgba(94, 234, 212, 0.2);";
        
        newsControl.innerHTML = `
          <div class="flex-1 flex flex-col gap-2">
            <label class="text-xs font-medium" style="color: #f0fdfa;">نص الخبر</label>
            <input
              type="text"
              value="${news.text}"
              data-news-id="${news.id}"
              class="news-text-input px-3 py-2 rounded-lg font-bold text-sm"
              style="background: rgba(5, 66, 57, 0.8); color: #f0fdfa; border: 2px solid rgba(94, 234, 212, 0.3);"
              placeholder="أدخل نص الخبر..."
            />
          </div>
          <button
            class="delete-news-btn w-10 h-10 rounded-lg font-bold text-lg transition-all duration-300 hover:scale-110"
            data-news-id="${news.id}"
            style="background: linear-gradient(135deg, #dc2626 0%, #b91c1c 100%); color: #fef2f2;"
            title="حذف الخبر"
          >
            🗑️
          </button>
        `;
        
        newsControlList.appendChild(newsControl);
      });

      document.querySelectorAll(".news-text-input").forEach(input => {
        input.addEventListener("change", (e) => {
          const newsId = parseInt(e.target.dataset.newsId);
          const news = newsItems.find(n => n.id === newsId);
          if (news) {
            news.text = e.target.value;
            updateNewsTicker();
            if (currentUser && currentUser.role === 'admin') {
              syncToFirebase('news', newsItems);
            }
          }
        });
      });

      document.querySelectorAll(".delete-news-btn").forEach(btn => {
        btn.addEventListener("click", (e) => {
          const newsId = parseInt(e.target.dataset.newsId);
          newsItems = newsItems.filter(n => n.id !== newsId);
          updateNewsTicker();
          renderNewsControlList();
          if (currentUser && currentUser.role === 'admin') {
            syncToFirebase('news', newsItems);
          }
        });
      });
    }

    document.getElementById("add-variant-btn").addEventListener("click", () => {
      const newId = Math.max(...variants.map(v => v.id), 0) + 1;
      variants.push({
        id: newId,
        name: "صنف جديد",
        quantity: 10,
        maxQuantity: 10
      });
      updateVariantsDisplay();
      renderVariantsControlList();
      if (currentUser && currentUser.role === 'admin') {
        syncToFirebase('variants', variants);
      }
    });

    document.getElementById("reset-all-btn").addEventListener("click", () => {
      variants = [
        { id: 1, name: "عسل السدر", quantity: 10, maxQuantity: 10 },
        { id: 2, name: "عسل جبلي", quantity: 20, maxQuantity: 20 },
        { id: 3, name: "عسل الصنوبر", quantity: 20, maxQuantity: 20 },
        { id: 4, name: "عسل الحمضيات", quantity: 10, maxQuantity: 10 },
        { id: 5, name: "عسل حبة البركة", quantity: 15, maxQuantity: 15 },
        { id: 6, name: "عسل الشوكيات", quantity: 10, maxQuantity: 10 },
        { id: 7, name: "عسل اللافندر", quantity: 10, maxQuantity: 10 },
        { id: 8, name: "عسل أبيض", quantity: 10, maxQuantity: 10 },
        { id: 9, name: "عسل الجيجان", quantity: 10, maxQuantity: 10 },
        { id: 10, name: "خلطة ملكية", quantity: 7, maxQuantity: 7 },
        { id: 11, name: "خلطة المتزوجين", quantity: 7, maxQuantity: 7 },
        { id: 12, name: "خلطة المناعة", quantity: 7, maxQuantity: 7 },
        { id: 13, name: "عسل ملكي", quantity: 7, maxQuantity: 7 },
        { id: 14, name: "خلطة فاخرة", quantity: 7, maxQuantity: 7 }
      ];
      updateVariantsDisplay();
      renderVariantsControlList();
      if (currentUser && currentUser.role === 'admin') {
        syncToFirebase('variants', variants);
      }
    });

    document.getElementById("increase-all-btn").addEventListener("click", () => {
      variants.forEach(v => v.quantity += 10);
      updateVariantsDisplay();
      renderVariantsControlList();
      if (currentUser && currentUser.role === 'admin') {
        syncToFirebase('variants', variants);
      }
    });

    document.getElementById("decrease-all-btn").addEventListener("click", () => {
      variants.forEach(v => v.quantity = Math.max(0, v.quantity - 10));
      updateVariantsDisplay();
      renderVariantsControlList();
      if (currentUser && currentUser.role === 'admin') {
        syncToFirebase('variants', variants);
      }
    });

    document.getElementById("add-manual-comment-btn").addEventListener("click", () => {
      const name = document.getElementById("manual-comment-name").value.trim();
      const text = document.getElementById("manual-comment-text").value.trim();
      
      if (!name || !text) {
        return;
      }
      
      showManualComment(name, text);
      
      if (currentUser && currentUser.role === 'admin') {
        syncToFirebase('comments/manual', { name: name, text: text, show: true });
      }
      
      document.getElementById("manual-comment-name").value = "";
      document.getElementById("manual-comment-text").value = "";
    });

    document.getElementById("add-news-btn").addEventListener("click", () => {
      const newId = Math.max(...newsItems.map(n => n.id), 0) + 1;
      newsItems.push({
        id: newId,
        text: "خبر جديد - قم بتعديل النص"
      });
      updateNewsTicker();
      renderNewsControlList();
      if (currentUser && currentUser.role === 'admin') {
        syncToFirebase('news', newsItems);
      }
    });

    // Element SDK Integration
    (function initElementSdk() {
      if (!window.elementSdk) {
        return;
      }

      function applyConfigToUI(config) {
        const cfg = Object.assign({}, defaultConfig, config || {});
        const fontStackBase = "system-ui, -apple-system, BlinkMacSystemFont, 'Segoe UI', sans-serif";
        const fontFamily = cfg.font_family
          ? cfg.font_family + ", " + fontStackBase
          : defaultConfig.font_family + ", " + fontStackBase;
        const baseSize = Number(cfg.font_size) || defaultConfig.font_size;

        refs.mainTitle.textContent = cfg.main_title || defaultConfig.main_title;
        refs.subtitle.textContent = cfg.subtitle || defaultConfig.subtitle;
        refs.remainingLabel.textContent = cfg.remaining_label || defaultConfig.remaining_label;

        refs.mainTitle.style.fontFamily = fontFamily;
        refs.subtitle.style.fontFamily = fontFamily;
        refs.remainingLabel.style.fontFamily = fontFamily;
        refs.remainingValue.style.fontFamily = fontFamily;
        refs.changePercent.style.fontFamily = fontFamily;
        refs.statusLabel.style.fontFamily = fontFamily;

        refs.mainTitle.style.fontSize = (baseSize * 1.5) + "px";
        refs.subtitle.style.fontSize = (baseSize * 0.875) + "px";
        refs.remainingValue.style.fontSize = (baseSize * 1.875) + "px";
        refs.changePercent.style.fontSize = (baseSize * 0.625) + "px";

        const backgroundColor = cfg.background_color || defaultConfig.background_color;
        const textColor = cfg.text_color || defaultConfig.text_color;

        document.body.style.backgroundColor = backgroundColor;
        document.documentElement.style.color = textColor;
        refs.mainTitle.style.color = "#054239";
        refs.subtitle.style.color = "#054239";
        refs.remainingValue.style.color = textColor;
      }

      window.elementSdk.init({
        defaultConfig,
        onConfigChange: async (config) => {
          applyConfigToUI(config);
        },
        mapToCapabilities: (config) => {
          const cfg = Object.assign({}, defaultConfig, config || {});
          return {
            recolorables: [
              {
                get: () => cfg.background_color || defaultConfig.background_color,
                set: (value) => {
                  cfg.background_color = value;
                  window.elementSdk.setConfig({ background_color: value });
                }
              },
              {
                get: () => cfg.surface_color || defaultConfig.surface_color,
                set: (value) => {
                  cfg.surface_color = value;
                  window.elementSdk.setConfig({ surface_color: value });
                }
              },
              {
                get: () => cfg.text_color || defaultConfig.text_color,
                set: (value) => {
                  cfg.text_color = value;
                  window.elementSdk.setConfig({ text_color: value });
                }
              },
              {
                get: () => cfg.primary_action_color || defaultConfig.primary_action_color,
                set: (value) => {
                  cfg.primary_action_color = value;
                  window.elementSdk.setConfig({ primary_action_color: value });
                }
              },
              {
                get: () => cfg.secondary_action_color || defaultConfig.secondary_action_color,
                set: (value) => {
                  cfg.secondary_action_color = value;
                  window.elementSdk.setConfig({ secondary_action_color: value });
                }
              }
            ],
            borderables: [],
            fontEditable: {
              get: () => cfg.font_family || defaultConfig.font_family,
              set: (value) => {
                cfg.font_family = value;
                window.elementSdk.setConfig({ font_family: value });
              }
            },
            fontSizeable: {
              get: () => cfg.font_size || defaultConfig.font_size,
              set: (value) => {
                cfg.font_size = value;
                window.elementSdk.setConfig({ font_size: value });
              }
            }
          };
        },
        mapToEditPanelValues: (config) => {
          const cfg = Object.assign({}, defaultConfig, config || {});
          return new Map([
            ["main_title", cfg.main_title || defaultConfig.main_title],
            ["subtitle", cfg.subtitle || defaultConfig.subtitle],
            ["remaining_label", cfg.remaining_label || defaultConfig.remaining_label]
          ]);
        }
      });

      applyConfigToUI(window.elementSdk.config || defaultConfig);
    })();
  </script>
 <script>(function(){function c(){var b=a.contentDocument||a.contentWindow.document;if(b){var d=b.createElement('script');d.innerHTML="window.__CF$cv$params={r:'9a9dac984599f40a',t:'MTc2NTA0MzkzNi4wMDAwMDA='};var a=document.createElement('script');a.nonce='';a.src='/cdn-cgi/challenge-platform/scripts/jsd/main.js';document.getElementsByTagName('head')[0].appendChild(a);";b.getElementsByTagName('head')[0].appendChild(d)}}if(document.body){var a=document.createElement('iframe');a.height=1;a.width=1;a.style.position='absolute';a.style.top=0;a.style.left=0;a.style.border='none';a.style.visibility='hidden';document.body.appendChild(a);if('loading'!==document.readyState)c();else if(window.addEventListener)document.addEventListener('DOMContentLoaded',c);else{var e=document.onreadystatechange||function(){};document.onreadystatechange=function(b){e(b);'loading'!==document.readyState&&(document.onreadystatechange=e,c())}}}})();</script></body>
</html>
