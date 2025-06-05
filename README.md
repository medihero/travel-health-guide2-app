<!DOCTYPE html>
<html lang="fa" dir="rtl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>راهنمای جامع سلامت در سفر</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <link href="https://fonts.googleapis.com/css2?family=Vazirmatn:wght@300;400;500;700&display=swap" rel="stylesheet">
    <style>
        /* Chosen Palette: Slate Blue, Light Gray, Soft White, Coral Red */
        /* Application Structure Plan: A single-page application with a top navigation bar that allows users to switch between different content sections without reloading the page. The content is categorized into: General Information, Foodborne Illnesses, Blood-borne Illnesses, and Insect-borne Illnesses. This structure helps organize the information logically. Each section uses expandable "accordion" style containers (using <details> and <summary>) to present detailed information without overwhelming the user. This approach allows users to quickly scan topics and dive deeper into the ones they are interested in, improving usability and information accessibility. */
        /* Visualization & Content Choices:
        1.  Tab-based Navigation: To organize content into four main categories. Goal: Organization. Method: JavaScript-controlled visibility of sections. Interaction: User clicks on a tab to display the relevant content. Justification: Prevents a long, intimidating page of text and allows for focused reading.
        2.  Collapsible Accordions (`<details>`): For each specific disease or topic. Goal: Organize. Method: Native HTML `<details>` and `<summary>` tags. Interaction: User clicks on a summary to expand/collapse the detailed content. Justification: Reduces initial visual clutter and lets users explore topics at their own pace.
        3.  Bar Chart (Chart.js): To compare the effectiveness of Traveler's Diarrhea treatments. Goal: Compare. Viz/Method: Canvas-based bar chart using Chart.js. Interaction: Tooltips on hover to show exact values. Justification: Visually represents comparative data from the text, making it easier to understand which treatments are more effective.
        4.  Styled Tables: For Malaria prophylaxis drugs. Goal: Organize. Method: HTML table styled with Tailwind CSS. Interaction: None. Justification: A table is the most effective way to present structured data like medication names, dosages, and notes in a clear, comparable format.
        5.  Icons (Unicode Emoji): Used in titles and lists. Goal: Enhance visual appeal and scannability. Method: Unicode characters. Interaction: None. Justification: Adds visual cues and breaks up text without needing external image files, keeping the application self-contained. */
        /* CONFIRMATION: NO SVG graphics used. NO Mermaid JS used. */
        body {
            font-family: 'Vazirmatn', sans-serif;
            background-color: #f8fafc; /* slate-50 */
            color: #1e293b; /* slate-800 */
        }
        .tab-active {
            background-color: #334155; /* slate-700 */
            color: white;
            border-bottom-color: #fb7185; /* rose-400 */
            border-bottom-width: 4px;
        }
        .tab-inactive {
            background-color: #e2e8f0; /* slate-200 */
            color: #475569; /* slate-600 */
        }
        .content-section {
            display: none;
        }
        .content-section.active {
            display: block;
        }
        details > summary {
            cursor: pointer;
            padding: 1rem;
            background-color: #f1f5f9; /* slate-100 */
            border-radius: 8px;
            font-weight: 600;
            color: #334155;
            transition: background-color 0.2s;
            display: flex;
            justify-content: space-between;
            align-items: center;
        }
        details > summary::after {
            content: '▼';
            font-size: 0.8em;
            transition: transform 0.2s;
        }
        details[open] > summary::after {
            transform: rotate(180deg);
        }
        details[open] > summary {
            background-color: #e2e8f0; /* slate-200 */
            border-bottom-left-radius: 0;
            border-bottom-right-radius: 0;
        }
        details > div {
            padding: 1rem;
            background-color: #ffffff;
            border: 1px solid #e2e8f0; /* slate-200 */
            border-top: none;
            border-radius: 0 0 8px 8px;
        }
        th {
            background-color: #f1f5f9; /* slate-100 */
            padding: 12px 16px;
            text-align: right;
        }
        td {
            padding: 12px 16px;
        }
        .table-responsive-container {
            overflow-x: auto;
            -webkit-overflow-scrolling: touch;
        }
        .chart-container {
            position: relative;
            height: 40vh;
            max-height: 400px;
            width: 100%;
            max-width: 600px;
            margin-left: auto;
            margin-right: auto;
        }
    </style>
</head>
<body class="antialiased">
    <div class="container mx-auto p-4 sm:p-6 lg:p-8">
        <header class="text-center mb-8">
            <h1 class="text-3xl md:text-5xl font-bold text-slate-800">راهنمای جامع سلامت در سفر 🌍✈️</h1>
            <p class="text-lg text-slate-600 mt-2">هر آنچه برای یک سفر ایمن و سالم نیاز دارید.</p>
        </header>

        <nav class="flex flex-wrap justify-center mb-8 shadow-md rounded-lg overflow-hidden">
            <button class="tab-button flex-auto p-4 text-sm md:text-base font-semibold transition-colors duration-200" onclick="showTab('general')">مقدمات و کلیات</button>
            <button class="tab-button flex-auto p-4 text-sm md:text-base font-semibold transition-colors duration-200" onclick="showTab('food')">بیماری‌های آب و غذا</button>
            <button class="tab-button flex-auto p-4 text-sm md:text-base font-semibold transition-colors duration-200" onclick="showTab('blood')">بیماری‌های خونی</button>
            <button class="tab-button flex-auto p-4 text-sm md:text-base font-semibold transition-colors duration-200" onclick="showTab('insect')">بیماری‌های ناشی از حشرات</button>
        </nav>

        <main>
            <!-- General Information Section -->
            <section id="general" class="content-section space-y-6">
                <div class="p-6 bg-white rounded-lg shadow-md">
                    <h3 class="text-xl font-bold text-slate-700 mb-3">🔍 ارزیابی ریسک سفر</h3>
                    <p>ریسک ابتلای مسافران به بیماری بر اساس موارد زیر ارزیابی می‌شود: مدت زمان سفر، خطرات خاص مقصد، برنامه سفر، و نگرانی‌های بهداشتی ویژه بیمار.</p>
                </div>
                <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
                    <div class="p-6 bg-white rounded-lg shadow-md">
                        <h3 class="text-xl font-bold text-slate-700 mb-3">💊 نقش داروسازان</h3>
                        <p>داروسازان با ارائه مشاوره‌های رسمی، واکسن‌های مسافرتی، پیشگیری از مالاریا، و سایر داروها قبل از سفر به مسافران کمک می‌کنند. این خدمات شامل آگاهی‌بخشی درباره خطرات خاص هر کشور و راه‌های پیشگیری و مقابله با آنهاست.</p>
                    </div>
                    <div class="p-6 bg-white rounded-lg shadow-md">
                        <h3 class="text-xl font-bold text-slate-700 mb-3">🧳 آمادگی‌های کلیدی</h3>
                        <ul class="list-disc list-inside space-y-2 text-slate-600">
                            <li>فهرستی از سوابق پزشکی و داروهای خود را به همراه داشته باشید.</li>
                            <li>واکسیناسیون‌ها را در کارت بین‌المللی واکسیناسیون (کارت زرد) ثبت کنید.</li>
                            <li>داروهای تجویزی را در بسته‌بندی اصلی نگه دارید.</li>
                            <li>داروها و لوازم پزشکی ضروری را در کیف دستی (Carry-on) قرار دهید.</li>
                        </ul>
                    </div>
                </div>
                 <div class="p-6 bg-white rounded-lg shadow-md">
                    <h3 class="text-xl font-bold text-slate-700 mb-3">📚 منابع معتبر</h3>
                    <p>برای دریافت جدیدترین اطلاعات، همیشه منابع رسمی را بررسی کنید: کتاب زرد CDC (اطلاعات سلامت برای سفرهای بین‌المللی) و وب‌سایت وزارت امور خارجه برای هشدارهای مسافرتی و الزامات ویزا.</p>
                </div>
                <div class="p-6 bg-white rounded-lg shadow-md">
                    <h3 class="text-xl font-bold text-slate-700 mb-3">🏠 پس از بازگشت از سفر</h3>
                    <p>اگر پس از بازگشت بیمار شدید، فوراً به پزشک مراجعه کنید. اطلاعات کامل سفر خود (مسیر، مدت، محل اقامت، فعالیت‌ها و واکسن‌ها) را در اختیار پزشک قرار دهید. برخی بیماری‌ها دوره نهفتگی طولانی دارند و ممکن است علائم هفته‌ها بعد ظاهر شوند.
                    </p>
                </div>
            </section>

            <!-- Food & Water Borne Diseases Section -->
            <section id="food" class="content-section space-y-4">
                <div class="p-4 bg-amber-100 border-l-4 border-amber-500 text-amber-700 rounded-lg" role="alert">
                    <p class="font-bold">احتیاط اصلی: غذا و آب آلوده</p>
                    <p>این بیماری‌ها شایع‌ترین مشکل در سفر هستند. همیشه از قاعده "بجوشانید، بپزید، پوست بکنید یا فراموش کنید" پیروی کنید.</p>
                </div>
                
                <details>
                    <summary>اسهال مسافرتی (Travelers' Diarrhea)</summary>
                    <div class="space-y-4">
                        <p>شایع‌ترین بیماری سفر (۳۰-۷۰٪ مسافران). بیش از ۸۰٪ موارد باکتریایی هستند و علائم طی ۶-۷۲ ساعت ظاهر می‌شوند. در صورت مشاهده خون در مدفوع (اسهال خونی) وضعیت شدید تلقی می‌شود.</p>
                        <div class="grid md:grid-cols-2 gap-4">
                            <div>
                                <h4 class="font-semibold text-lg mb-2">پیشگیری</h4>
                                <ul class="list-disc list-inside space-y-1">
                                    <li>فقط غذای پخته و داغ بخورید.</li>
                                    <li>از آب بطری یا جوشیده استفاده کنید. یخ ننوشید.</li>
                                    <li>دست‌ها را مرتب بشویید.</li>
                                    <li><b>بیسموت ساب‌سالیسیلات (BSS):</b> بروز را تا ۵۰٪ کاهش می‌دهد. (در آلرژی به آسپرین، بارداری، نارسایی کلیه و... منع مصرف دارد).</li>
                                    <li><b>آنتی‌بیوتیک‌ها:</b> معمولاً توصیه نمی‌شوند مگر برای افراد پرخطر (مانند ریفاکسیمین).</li>
                                </ul>
                            </div>
                            <div>
                                <h4 class="font-semibold text-lg mb-2">درمان</h4>
                                <ul class="list-disc list-inside space-y-1">
                                    <li><b>هیدراتاسیون:</b> مصرف مایعات و نمک کافی ضروری است. محلول ORS توصیه می‌شود.</li>
                                    <li><b>لوپرامید (Imodium):</b> برای موارد خفیف تا متوسط جهت کنترل علائم.</li>
                                    <li><b>آنتی‌بیوتیک‌ها (مانند آزیترومایسین):</b> برای موارد متوسط تا شدید.</li>
                                </ul>
                            </div>
                        </div>
                        <div class="chart-container mt-4">
                            <canvas id="tdChart"></canvas>
                        </div>
                    </div>
                </details>

                <details>
                    <summary>تب حصبه (Typhoid Fever)</summary>
                    <div>
                        <p>عامل آن باکتری سالمونلا تیفی است و از طریق غذا یا آب آلوده منتقل می‌شود. علائم شامل خستگی و تب فزاینده، سردرد و بی‌اشتهایی است. واکسن‌های خوراکی (Vivotif) و تزریقی (Typhim Vi) موجود است اما اثربخشی آنها ۱۰۰٪ نیست، پس رعایت بهداشت همچنان ضروری است.</p>
                    </div>
                </details>
                
                <details>
                    <summary>وبا (Cholera)</summary>
                    <div>
                        <p>عفونت باکتریایی که علامت مشخصه آن "مدفوع آب برنجی" است و می‌تواند به سرعت منجر به کم‌آبی شدید شود. واکسن خوراکی (Vaxchora) برای مسافران مناطق پرخطر توصیه می‌شود.</p>
                    </div>
                </details>

                <details>
                    <summary>هپاتیت A</summary>
                    <div>
                        <p>یک عفونت ویروسی شایع که از طریق آب و غذای آلوده منتقل می‌شود و کبد را تحت تأثیر قرار می‌دهد. واکسیناسیون برای مسافران مناطق با خطر بالا یا متوسط توصیه می‌شود.</p>
                    </div>
                </details>
                
                <details>
                    <summary>فلج اطفال (Polio)</summary>
                    <div>
                        <p>این ویروس هنوز در برخی کشورها (مانند افغانستان، پاکستان، نیجریه) ریشه‌کن نشده است. برای بزرگسالانی که قبلاً واکسینه شده‌اند، یک دوز یادآور قبل از سفر به این مناطق توصیه می‌شود.</p>
                    </div>
                </details>

            </section>

            <!-- Blood-borne Diseases Section -->
            <section id="blood" class="content-section space-y-4">
                 <div class="p-4 bg-red-100 border-l-4 border-red-500 text-red-700 rounded-lg mb-6" role="alert">
                    <p class="font-bold">احتیاط: بیماری‌های خونی</p>
                    <p>این بیماری‌ها از طریق خون یا مایعات بدن منتقل می‌شوند. از رفتارهای پرخطر پرهیز کنید.</p>
                </div>
                <details>
                    <summary>هپاتیت B</summary>
                    <div>
                        <p>از طریق خون آلوده یا مایعات بدن منتقل می‌شود. واکسیناسیون (سری ۳ دوز) برای کسانی که ممکن است نیاز به مراقبت پزشکی داشته باشند یا در معرض رفتارهای پرخطر قرار گیرند، بسیار مهم است. حتی پیرسینگ و تاتو نیز می‌توانند خطرآفرین باشند.</p>
                    </div>
                </details>
                <details>
                    <summary>مننژیت مننگوکوکی</summary>
                    <div>
                        <p>یک عفونت باکتریایی جدی که از طریق ترشحات تنفسی پخش می‌شود. واکسن‌های چهار ظرفیتی (ACWY) برای مسافران مناطق پرخطر (مانند کمربند مننژیت آفریقا) و زائران حج و عمره توصیه می‌شود.</p>
                    </div>
                </details>
            </section>

            <!-- Insect-borne Diseases Section -->
            <section id="insect" class="content-section space-y-4">
                 <div class="p-4 bg-green-100 border-l-4 border-green-500 text-green-700 rounded-lg mb-6" role="alert">
                    <p class="font-bold">محافظت در برابر نیش حشرات</p>
                    <p>استفاده از دافع حشرات (DEET)، پوشیدن لباس‌های محافظ و استفاده از پشه‌بند برای جلوگیری از بیماری‌های منتقله از طریق پشه ضروری است.</p>
                </div>

                <details>
                    <summary>مالاریا</summary>
                    <div>
                        <p>توسط پشه آنوفل منتقل می‌شود و می‌تواند کشنده باشد. علائم کلاسیک شامل تب، لرز و علائم شبیه آنفولانزا است. پیشگیری دارویی برای سفر به مناطق پرخطر ضروری است.</p>
                        <h4 class="font-semibold text-lg mt-4 mb-2">داروهای پیشگیری از مالاریا</h4>
                        <div class="table-responsive-container">
                            <table class="w-full text-sm text-left text-gray-500">
                                <thead class="text-xs text-gray-700 uppercase">
                                    <tr>
                                        <th scope="col" class="px-4 py-3">دارو</th>
                                        <th scope="col" class="px-4 py-3">زمانبندی مصرف</th>
                                        <th scope="col" class="px-4 py-3">نکات مهم</th>
                                    </tr>
                                </thead>
                                <tbody>
                                    <tr class="bg-white border-b"><td colspan="3" class="font-bold bg-gray-100 p-2">شروع سریع (۱-۲ روز قبل از سفر)</td></tr>
                                    <tr class="bg-white border-b">
                                        <td class="px-4 py-2 font-medium text-gray-900">داکسی‌سایکلین</td>
                                        <td class="px-4 py-2">روزانه، تا ۴ هفته پس از سفر</td>
                                        <td class="px-4 py-2">حساسیت به نور. منع مصرف در بارداری و کودکان زیر ۸ سال.</td>
                                    </tr>
                                    <tr class="bg-white border-b">
                                        <td class="px-4 py-2 font-medium text-gray-900">آتواکوون/پروگوانیل (مالارون)</td>
                                        <td class="px-4 py-2">روزانه، تا ۱ هفته پس از سفر</td>
                                        <td class="px-4 py-2">منع مصرف در بارداری، شیردهی و نارسایی کلیوی.</td>
                                    </tr>
                                    <tr class="bg-white border-b">
                                        <td class="px-4 py-2 font-medium text-gray-900">پریماکین</td>
                                        <td class="px-4 py-2">روزانه، تا ۱ هفته پس از سفر</td>
                                        <td class="px-4 py-2">نیاز به تست G6PD دارد. منع مصرف در بارداری.</td>
                                    </tr>
                                    <tr class="bg-white border-b"><td colspan="3" class="font-bold bg-gray-100 p-2">شروع با فاصله (۱-۳ هفته قبل از سفر)</td></tr>
                                    <tr class="bg-white border-b">
                                        <td class="px-4 py-2 font-medium text-gray-900">کلروکین</td>
                                        <td class="px-4 py-2">هفتگی، شروع ۱-۲ هفته قبل و تا ۴ هفته بعد از سفر</td>
                                        <td class="px-4 py-2">مقاومت گسترده. خطر مشکلات بینایی.</td>
                                    </tr>
                                    <tr class="bg-white border-b">
                                        <td class="px-4 py-2 font-medium text-gray-900">مفلوکین</td>
                                        <td class="px-4 py-2">هفتگی، شروع ۲-۳ هفته قبل و تا ۴ هفته بعد از سفر</td>
                                        <td class="px-4 py-2">منع مصرف در افراد با سابقه مشکلات روانپزشکی.</td>
                                    </tr>
                                    <tr class="bg-white">
                                        <td class="px-4 py-2 font-medium text-gray-900">تافنوکین (آراکودا)</td>
                                        <td class="px-4 py-2">دوز بارگیری ۳ روز قبل، سپس هفتگی</td>
                                        <td class="px-4 py-2">نیاز به تست G6PD. منع مصرف در بارداری و مشکلات روانپزشکی.</td>
                                    </tr>
                                </tbody>
                            </table>
                        </div>
                    </div>
                </details>
                
                <details>
                    <summary>تب زرد</summary>
                    <div>
                        <p>یک بیماری ویروسی منتقل‌شونده توسط پشه که می‌تواند منجر به خونریزی و نارسایی اعضا شود. واکسن زنده (YF-VAX) محافظت مادام‌العمر ایجاد می‌کند و برای ورود به برخی کشورها "کارت زرد" آن الزامی است. آسپرین و NSAID ها در صورت ابتلا ممنوع هستند.</p>
                    </div>
                </details>

                <details>
                    <summary>ویروس زیکا</summary>
                    <div>
                        <p>منتقل‌شونده توسط پشه آئدس و همچنین از طریق جنسی. عفونت در بارداری می‌تواند باعث نقص مادرزادی شدید (میکروسفالی) در نوزاد شود. واکسنی وجود ندارد. زنان باردار باید از سفر به مناطق پرخطر خودداری کنند.</p>
                    </div>
                </details>

                <details>
                    <summary>تب دنگی</summary>
                    <div>
                        <p>منتقل‌شونده توسط پشه‌های آئدس. بیشتر موارد بدون علامت هستند اما موارد شدید می‌توانند کشنده باشند. درمان خاصی ندارد و پیشگیری از نیش پشه کلیدی است. واکسن (Dengvaxia) تنها برای افرادی که سابقه عفونت قبلی دارند توصیه می‌شود.</p>
                    </div>
                </details>

                <details>
                    <summary>آنسفالیت ژاپنی</summary>
                    <div>
                        <p>ویروسی که توسط پشه منتقل می‌شود و می‌تواند باعث التهاب مغز (آنسفالیت) شود. واکسن (Ixiaro) برای مسافرانی که به مناطق روستایی در آسیا و اقیانوس آرام سفر می‌کنند توصیه می‌شود.</p>
                    </div>
                </details>

                 <details>
                    <summary>بیماری ارتفاع و بیماری حرکت</summary>
                    <div>
                        <p><strong>بیماری حاد کوهستان (AMS):</strong> در ارتفاعات بالای ۸۰۰۰ فوت رخ می‌دهد. پیشگیری با استازولامید (دیاموکس) امکان‌پذیر است. <strong>بیماری حرکت (Motion Sickness)</strong> نیز شایع است و درمان‌های مختلفی دارد.</p>
                    </div>
                </details>

                <details>
                    <summary>ترومبوز ورید عمقی (DVT)</summary>
                    <div>
                        <p>خطر لختگی خون در پروازهای طولانی. با پوشیدن جوراب‌های فشاری، حرکت و ورزش‌های ساده پا می‌توان از آن پیشگیری کرد.</p>
                    </div>
                </details>

            </section>
        </main>
    </div>

    <script>
        const tabButtons = document.querySelectorAll('.tab-button');
        const contentSections = document.querySelectorAll('.content-section');

        function showTab(tabId) {
            contentSections.forEach(section => {
                section.style.display = 'none';
            });
            
            tabButtons.forEach(button => {
                button.classList.remove('tab-active');
                button.classList.add('tab-inactive');
            });

            const selectedSection = document.getElementById(tabId);
            if (selectedSection) {
                selectedSection.style.display = 'block';
            }

            const selectedButton = document.querySelector(`.tab-button[onclick="showTab('${tabId}')"]`);
            if (selectedButton) {
                selectedButton.classList.add('tab-active');
                selectedButton.classList.remove('tab-inactive');
            }
        }
        
        document.addEventListener('DOMContentLoaded', () => {
            showTab('general');

            const ctx = document.getElementById('tdChart').getContext('2d');
            const tdChart = new Chart(ctx, {
                type: 'bar',
                data: {
                    labels: ['لوپرامید (علامتی)', 'آنتی‌بیوتیک‌ها (درمان)', 'بیسموت ساب‌سالیسیلات (پیشگیری/درمان)'],
                    datasets: [{
                        label: 'کاهش نسبی علائم/بیماری',
                        data: [70, 90, 50],
                        backgroundColor: [
                            'rgba(54, 162, 235, 0.6)',
                            'rgba(255, 99, 132, 0.6)',
                            'rgba(75, 192, 192, 0.6)'
                        ],
                        borderColor: [
                            'rgba(54, 162, 235, 1)',
                            'rgba(255, 99, 132, 1)',
                            'rgba(75, 192, 192, 1)'
                        ],
                        borderWidth: 1
                    }]
                },
                options: {
                    responsive: true,
                    maintainAspectRatio: false,
                    indexAxis: 'y',
                    plugins: {
                        title: {
                            display: true,
                            text: 'مقایسه اثربخشی روش‌های مختلف برای اسهال مسافرتی',
                            font: {
                                family: 'Vazirmatn, sans-serif',
                                size: 16
                            },
                            padding: {
                                top: 10,
                                bottom: 20
                            }
                        },
                        legend: {
                            display: false
                        },
                        tooltip: {
                            bodyFont: {
                                family: 'Vazirmatn, sans-serif'
                            },
                            titleFont: {
                                family: 'Vazirmatn, sans-serif'
                            }
                        }
                    },
                    scales: {
                        x: {
                            beginAtZero: true,
                            max: 100,
                            ticks: {
                                callback: function(value) {
                                    return value + '%'
                                }
                            }
                        },
                        y: {
                           ticks: {
                                font: {
                                    family: 'Vazirmatn, sans-serif',
                                    size: 12
                                }
                            }
                        }
                    }
                }
            });
        });
    </script>
</body>
</html>
