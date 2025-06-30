<!DOCTYPE html>
<html lang="fa" dir="rtl">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>بنر ساز</title>
  <script src="https://cdn.tailwindcss.com"></script>
  <script src="https://html2canvas.hertzen.com/dist/html2canvas.min.js"></script>
  <style>
    @font-face {
      font-family: 'Vazir';
      src: url('https://cdn.fontcdn.ir/Font/Persian/Vazir/Vazir.ttf');
    }
    #bannerPreview {
      background-size: cover;
      background-position: center;
    }
    #aboutModal {
      display: none;
    }
    #aboutModal.show {
      display: flex;
    }
  </style>
</head>
<body class="bg-gray-100 min-h-screen flex items-center justify-center p-4">
  <div class="bg-white rounded-lg shadow-lg p-6 max-w-lg w-full">
    <div class="flex justify-between items-center mb-4">
      <h1 class="text-2xl font-bold text-center">بنر ساز</h1>
      <button id="aboutBtn" class="bg-gray-200 text-gray-700 px-3 py-1 rounded-md hover:bg-gray-300">درباره</button>
    </div>
    <div class="space-y-4">
      <div>
        <label for="bannerText" class="block text-sm font-medium text-gray-700">متن بنر</label>
        <input id="bannerText" type="text" class="mt-1 w-full p-2 border rounded-md" placeholder="متن خود را وارد کنید" value="بنر نمونه">
      </div>
      <div>
        <label for="bgColor" class="block text-sm font-medium text-gray-700">رنگ پس‌زمینه (اگر تصویر انتخاب نشود)</label>
        <input id="bgColor" type="color" class="mt-1 w-full h-10 border rounded-md" value="#2563eb">
      </div>
      <div>
        <label for="bgImage" class="block text-sm font-medium text-gray-700">انتخاب تصویر پس‌زمینه</label>
        <input id="bgImage" type="file" accept="image/*" class="mt-1 w-full p-2 border rounded-md">
      </div>
      <div>
        <label for="textColor" class="block text-sm font-medium text-gray-700">رنگ متن</label>
        <input id="textColor" type="color" class="mt-1 w-full h-10 border rounded-md" value="#ffffff">
      </div>
      <div>
        <label for="fontSize" class="block text-sm font-medium text-gray-700">اندازه فونت (پیکسل)</label>
        <input id="fontSize" type="number" class="mt-1 w-full p-2 border rounded-md" value="24" min="10" max="100">
      </div>
      <div>
        <label for="fontFamily" class="block text-sm font-medium text-gray-700">نوع فونت</label>
        <select id="fontFamily" class="mt-1 w-full p-2 border rounded-md">
          <option value="Vazir">وزیر</option>
          <option value="Arial">Arial</option>
          <option value="Times New Roman">Times New Roman</option>
        </select>
      </div>
      <button id="downloadBtn" class="w-full bg-blue-600 text-white p-2 rounded-md hover:bg-blue-700">دانلود بنر</button>
    </div>
    <div id="bannerPreview" class="mt-6 w-full h-40 flex items-center justify-center text-center" style="background-color: #2563eb; color: #ffffff; font-family: Vazir; font-size: 24px;">
      بنر نمونه
    </div>
  </div>

  <!-- Modal برای درباره -->
  <div id="aboutModal" class="fixed inset-0 bg-black bg-opacity-50 flex items-center justify-center">
    <div class="bg-white rounded-lg p-6 max-w-sm w-full">
      <h2 class="text-xl font-bold mb-4">درباره بنر ساز</h2>
      <p class="text-gray-700 mb-2">ایمیل: mahdishamiahar@gmil.com</p>
      <p class="text-gray-700 mb-2">توسعه‌دهنده: مهدی شامی</p>
      <p class="text-gray-700 mb-4">شماره: 09142500486</p>
      <button id="closeModalBtn" class="w-full bg-gray-200 text-gray-700 p-2 rounded-md hover:bg-gray-300">بستن</button>
    </div>
  </div>

  <script>
    const bannerText = document.getElementById('bannerText');
    const bgColor = document.getElementById('bgColor');
    const bgImage = document.getElementById('bgImage');
    const textColor = document.getElementById('textColor');
    const fontSize = document.getElementById('fontSize');
    const fontFamily = document.getElementById('fontFamily');
    const bannerPreview = document.getElementById('bannerPreview');
    const downloadBtn = document.getElementById('downloadBtn');
    const aboutBtn = document.getElementById('aboutBtn');
    const aboutModal = document.getElementById('aboutModal');
    const closeModalBtn = document.getElementById('closeModalBtn');

    // تغییرات بنر
    function updateBanner() {
      bannerPreview.textContent = bannerText.value;
      bannerPreview.style.backgroundColor = bgColor.value;
      bannerPreview.style.color = textColor.value;
      bannerPreview.style.fontFamily = fontFamily.value;
      bannerPreview.style.fontSize = fontSize.value + "px";
    }

    // تصویر پس‌زمینه
    bgImage.addEventListener('change', function(e) {
      const file = e.target.files[0];
      if (file) {
        const reader = new FileReader();
        reader.onload = function(event) {
          bannerPreview.style.backgroundImage = "url('" + event.target.result + "')";
        }
        reader.readAsDataURL(file);
      } else {
        bannerPreview.style.backgroundImage = "";
      }
    });

    // رویدادها
    bannerText.addEventListener('input', updateBanner);
    bgColor.addEventListener('input', function() {
      bannerPreview.style.backgroundImage = "";
      updateBanner();
    });
    textColor.addEventListener('input', updateBanner);
    fontSize.addEventListener('input', updateBanner);
    fontFamily.addEventListener('change', updateBanner);

    // دانلود بنر
    downloadBtn.addEventListener('click', function() {
      html2canvas(bannerPreview, {useCORS: true}).then(function(canvas) {
        let link = document.createElement('a');
        link.download = 'banner.png';
        link.href = canvas.toDataURL();
        link.click();
      });
    });

    // درباره
    aboutBtn.addEventListener('click', function() {
      aboutModal.classList.add('show');
    });
    closeModalBtn.addEventListener('click', function() {
      aboutModal.classList.remove('show');
    });

    // بارگذاری اولیه
    updateBanner();
  </script>
</body>
</html>
