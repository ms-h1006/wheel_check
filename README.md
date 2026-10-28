<!DOCTYPE html>
<html lang="ja">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>アルミホイール売却判断ナビ | スクラップ買取 vs ヤフオク損益分岐点</title>
  <meta name="description" content="アルミホイールをスクラップ持ち込み買取で売るか、ヤフオクで販売すべきかを自動判定する損益分岐点計算ツール。">
  <!-- Tailwind CSS CDN -->
  <script src="https://cdn.tailwindcss.com"></script>
  <script>
    tailwind.config = {
      theme: {
        extend: {
          colors: {
            brand: {
              50: '#f0f7ff',
              100: '#e0effe',
              500: '#0284c7',
              600: '#0369a1',
              700: '#075985',
              800: '#0c4a6e',
              900: '#082f49',
            }
          }
        }
      }
    }
  </script>
  <style>
    input[type=number]::-webkit-inner-spin-button, 
    input[type=number]::-webkit-outer-spin-button { 
      -webkit-appearance: none; 
      margin: 0; 
    }
    input[type=number] {
      -moz-appearance: textfield;
    }
  </style>
</head>
<body class="bg-slate-50 text-slate-800 antialiased min-h-screen flex flex-col font-sans selection:bg-sky-100 selection:text-sky-900">

  <!-- Header -->
  <header class="bg-slate-900 text-white border-b border-slate-800 sticky top-0 z-30 shadow-sm">
    <div class="max-w-4xl mx-auto px-4 py-3.5 flex items-center justify-between">
      <div class="flex items-center gap-2.5">
        <div class="w-8 h-8 rounded-lg bg-sky-500/20 text-sky-400 flex items-center justify-center font-bold text-sm border border-sky-400/30">
          Al
        </div>
        <div>
          <h1 class="text-base font-bold tracking-tight text-white leading-tight">アルミホイール売却判断ナビ</h1>
          <p class="text-[11px] text-slate-400">スクラップ持ち込み買取 vs ヤフオク損益分岐点</p>
        </div>
      </div>
      <button id="resetBtn" type="button" class="text-xs text-slate-300 hover:text-white px-2.5 py-1.5 rounded bg-slate-800 hover:bg-slate-700 transition flex items-center gap-1 border border-slate-700">
        <svg class="w-3.5 h-3.5" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 4v5h.582m15.356 2A8.001 8.001 0 004.582 9m0 0H9m11 11v-5h-.581m0 0a8.003 8.003 0 01-15.357-2m15.357 2H15"/></svg>
        リセット
      </button>
    </div>
  </header>

  <!-- Main Content -->
  <main class="flex-1 max-w-4xl w-full mx-auto px-4 py-5 md:py-8 space-y-6">

    <!-- Comparison Summary Cards (Large Real-Time Result) -->
    <div class="grid grid-cols-1 md:grid-cols-2 gap-4">
      
      <!-- Card 1: 持ち込み買取価格 -->
      <div class="bg-white rounded-2xl p-5 border border-slate-200 shadow-sm relative overflow-hidden flex flex-col justify-between">
        <div class="flex items-start justify-between">
          <div>
            <span class="text-xs font-semibold text-slate-500 tracking-wider">金属スクラップ回収</span>
            <h2 class="text-lg font-bold text-slate-900 mt-0.5">持ち込み買取価格</h2>
          </div>
          <span class="px-2 py-0.5 text-[11px] font-medium bg-slate-100 text-slate-600 rounded">
            即現金化・状態不問
          </span>
        </div>

        <div class="my-4">
          <div class="flex items-baseline gap-1">
            <span class="text-xs font-bold text-slate-500">¥</span>
            <span id="scrapTotalPriceDisplay" class="text-4xl md:text-5xl font-black text-slate-900 tracking-tight font-mono tabular-nums">0</span>
            <span class="text-xs font-semibold text-slate-500 ml-1">(税込見込)</span>
          </div>
          <p class="text-xs text-slate-500 mt-1 flex items-center gap-1.5 font-mono">
            <span>総重量: <strong id="totalWeightDisplay" class="text-slate-800 font-bold">0</strong> kg</span>
            <span class="text-slate-300">|</span>
            <span>単価: <span id="scrapUnitPriceDisplay" class="text-slate-700">450</span>円/kg</span>
          </p>
        </div>

        <div class="text-[11px] text-slate-500 bg-slate-50 p-2.5 rounded-xl border border-slate-100 flex items-center justify-between">
          <span>梱包・クレームリスク: ゼロ</span>
          <span class="text-emerald-600 font-semibold">即日現金受取</span>
        </div>
      </div>

      <!-- Card 2: ヤフオク損益分岐点 -->
      <div class="bg-gradient-to-br from-amber-500/10 via-amber-500/5 to-white rounded-2xl p-5 border-2 border-amber-400/80 shadow-sm relative overflow-hidden flex flex-col justify-between">
        <div class="flex items-start justify-between">
          <div>
            <span class="text-xs font-bold text-amber-700 tracking-wider">ヤフオク出品ボーダーライン</span>
            <h2 class="text-lg font-bold text-slate-900 mt-0.5">損益分岐点（最低落札額）</h2>
          </div>
          <span class="px-2 py-0.5 text-[11px] font-bold bg-amber-500 text-white rounded shadow-sm">
            この金額以上で利益
          </span>
        </div>

        <div class="my-4">
          <div class="flex items-baseline gap-1">
            <span class="text-xs font-bold text-amber-800">¥</span>
            <span id="breakEvenPriceDisplay" class="text-4xl md:text-5xl font-black text-amber-600 tracking-tight font-mono tabular-nums">0</span>
            <span class="text-xs font-semibold text-slate-600 ml-1">以上</span>
          </div>
          <p class="text-xs text-amber-900/80 mt-1">
            システム手数料10%<span id="shippingNoticeText"></span>を引いて買取額を超える境界線
          </p>
        </div>

        <div class="text-[11px] text-amber-900 bg-amber-50/80 p-2.5 rounded-xl border border-amber-200/60 leading-relaxed">
          💡 落札額が<strong>この金額未満なら持ち込み買取の方が手取りが多くお得</strong>です。
        </div>
      </div>
    </div>

    <!-- Quick Verdict Banner -->
    <div id="verdictBanner" class="p-4 rounded-xl bg-slate-900 text-white flex flex-col sm:flex-row sm:items-center justify-between gap-3 shadow-md">
      <div class="flex items-center gap-3">
        <div class="w-10 h-10 rounded-lg bg-sky-500 text-white flex items-center justify-center shrink-0">
          <svg class="w-6 h-6" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M13 7h8m0 0v8m0-8l-8 8-4-4-6 6"/></svg>
        </div>
        <div>
          <div class="text-xs text-sky-300 font-semibold">判定ガイドライン</div>
          <div id="verdictHeadline" class="text-sm font-bold text-white mt-0.5">
            ヤフオクで <span id="verdictPrice" class="text-amber-400 font-mono text-base underline decoration-amber-400 decoration-2">¥0</span> 以上で売れる見込みがあればヤフオク推奨！
          </div>
        </div>
      </div>
      <div class="text-[11px] text-slate-300 sm:text-right shrink-0">
        人気純正・有名ブランド・美品 ➔ <strong>ヤフオク</strong><br>
        傷多数・腐食・曲がり・不人気純正 ➔ <strong>即持ち込み買取</strong>
      </div>
    </div>

    <!-- Interactive Input Panel -->
    <div class="bg-white rounded-2xl p-5 md:p-6 border border-slate-200 shadow-sm space-y-6">
      <div class="flex items-center justify-between border-b border-slate-100 pb-3">
        <h3 class="text-base font-bold text-slate-900 flex items-center gap-2">
          <svg class="w-5 h-5 text-sky-600" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 6V4m0 2a2 2 0 100 4m0-4a2 2 0 110 4m-6 8a2 2 0 100-4m0 4a2 2 0 110-4m0 4v2m0-6V4m6 6v10m6-2a2 2 0 100-4m0 4a2 2 0 110-4m0 4v2m0-6V4"/></svg>
          ホイール情報の入力
        </h3>
        <span class="text-xs text-slate-400 font-medium">変更は即座に反映</span>
      </div>

      <!-- Grid for Inputs -->
      <div class="grid grid-cols-1 md:grid-cols-2 gap-5">
        
        <!-- 1. インチ数選択 -->
        <div>
          <label for="inchSelect" class="block text-xs font-bold text-slate-700 mb-1.5">
            ① インチ数
          </label>
          <div class="relative">
            <select id="inchSelect" class="w-full h-12 pl-3.5 pr-10 text-base font-semibold bg-slate-50 border border-slate-300 rounded-xl focus:bg-white focus:outline-none focus:ring-2 focus:ring-sky-500 focus:border-sky-500 transition cursor-pointer appearance-none">
              <option value="13">13 インチ (軽自動車・コンパクト)</option>
              <option value="14">14 インチ (軽・大衆車)</option>
              <option value="15" selected>15 インチ (標準乗用車・コンパクト)</option>
              <option value="16">16 インチ (ミニバン・SUV)</option>
              <option value="17">17 インチ (セダン・ミニバン)</option>
              <option value="18">18 インチ (スポーツ・大型ミニバン)</option>
              <option value="19">19 インチ (プレミアム・大径)</option>
              <option value="20">20 インチ (大径ラグジュアリー)</option>
              <option value="21">21 インチ (超大径SUV等)</option>
              <option value="22">22 インチ (超大径・カスタム)</option>
            </select>
            <div class="absolute inset-y-0 right-0 flex items-center px-3 pointer-events-none text-slate-500">
              <svg class="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M19 9l-7 7-7-7"/></svg>
            </div>
          </div>
        </div>

        <!-- 2. 種類選択 (純正 vs 社外) -->
        <div>
          <label class="block text-xs font-bold text-slate-700 mb-1.5">
            ② ホイールの種類
          </label>
          <div class="grid grid-cols-2 gap-2 h-12 p-1 bg-slate-100 rounded-xl">
            <button type="button" id="typeOEM" class="type-btn rounded-lg text-sm font-bold transition flex items-center justify-center gap-1.5 bg-white text-slate-900 shadow-sm border border-slate-200">
              <svg class="w-4 h-4 text-sky-600" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 12l2 2 4-4m5.618-4.016A11.955 11.955 0 0112 2.944a11.955 11.955 0 01-8.618 3.04A12.02 12.02 0 003 9c0 5.591 3.824 10.29 9 11.622 5.176-1.332 9-6.03 9-11.622 0-1.042-.133-2.052-.382-3.016z"/></svg>
              純正ホイール
            </button>
            <button type="button" id="typeAftermarket" class="type-btn rounded-lg text-sm font-semibold transition flex items-center justify-center gap-1.5 text-slate-600 hover:text-slate-900">
              <svg class="w-4 h-4 text-slate-400" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M5 3v4M3 5h4M6 17v4m-2-2h4m5-16l2.286 6.857L21 12l-5.714 2.143L13 21l-2.286-6.857L5 12l5.714-2.143L13 3z"/></svg>
              社外ホイール
            </button>
          </div>
        </div>

        <!-- 3. 本数 (＋/ーボタン) -->
        <div>
          <label class="block text-xs font-bold text-slate-700 mb-1.5">
            ③ 本数
          </label>
          <div class="flex items-center gap-2">
            <button type="button" id="countMinus" class="w-12 h-12 rounded-xl bg-slate-100 hover:bg-slate-200 active:scale-95 text-slate-700 font-bold text-xl flex items-center justify-center border border-slate-200 transition">
              −
            </button>
            <div class="relative flex-1">
              <input type="number" id="countInput" min="1" max="20" value="4" class="w-full h-12 text-center text-xl font-bold bg-slate-50 border border-slate-300 rounded-xl focus:bg-white focus:outline-none focus:ring-2 focus:ring-sky-500 font-mono">
              <span class="absolute right-3.5 top-3.5 text-xs text-slate-400 font-semibold pointer-events-none">本</span>
            </div>
            <button type="button" id="countPlus" class="w-12 h-12 rounded-xl bg-slate-100 hover:bg-slate-200 active:scale-95 text-slate-700 font-bold text-xl flex items-center justify-center border border-slate-200 transition">
              ＋
            </button>
          </div>
          <div class="flex items-center gap-1.5 mt-1.5">
            <button type="button" class="count-preset text-[11px] px-2 py-0.5 rounded bg-slate-100 hover:bg-slate-200 text-slate-600" data-count="1">1本</button>
            <button type="button" class="count-preset text-[11px] px-2 py-0.5 rounded bg-slate-100 hover:bg-slate-200 text-slate-600" data-count="2">2本</button>
            <button type="button" class="count-preset text-[11px] px-2 py-0.5 rounded bg-slate-100 hover:bg-slate-200 text-slate-600 font-semibold" data-count="4">4本 (1台分)</button>
          </div>
        </div>

        <!-- 4. 1本あたりの平均重量 (自動設定 & 微調整可能) -->
        <div>
          <div class="flex items-center justify-between mb-1.5">
            <label for="weightInput" class="text-xs font-bold text-slate-700">
              ④ 1本あたりの重量
            </label>
            <span id="weightSourceBadge" class="text-[10px] text-sky-700 bg-sky-50 px-1.5 py-0.5 rounded border border-sky-200">
              目安テーブル適用中
            </span>
          </div>
          <div class="flex items-center gap-2">
            <button type="button" id="weightMinus" class="w-12 h-12 rounded-xl bg-slate-100 hover:bg-slate-200 active:scale-95 text-slate-700 font-bold text-base flex items-center justify-center border border-slate-200 transition" title="0.5kg減らす">
              -0.5
            </button>
            <div class="relative flex-1">
              <input type="number" id="weightInput" step="0.1" min="1" max="40" value="8.5" class="w-full h-12 text-center text-xl font-bold bg-slate-50 border border-slate-300 rounded-xl focus:bg-white focus:outline-none focus:ring-2 focus:ring-sky-500 font-mono">
              <span class="absolute right-3.5 top-3.5 text-xs text-slate-400 font-semibold pointer-events-none">kg / 本</span>
            </div>
            <button type="button" id="weightPlus" class="w-12 h-12 rounded-xl bg-slate-100 hover:bg-slate-200 active:scale-95 text-slate-700 font-bold text-base flex items-center justify-center border border-slate-200 transition" title="0.5kg増やす">
              +0.5
            </button>
          </div>
          <p class="text-[11px] text-slate-400 mt-1">
            ※インチと種類から自動セット。タイヤ付きや鍛造品など実測に合わせて手動編集可能。
          </p>
        </div>

        <!-- 5. アルミのキロあたり買取価格 (初期値450円・上書き可) -->
        <div class="md:col-span-2">
          <div class="flex items-center justify-between mb-1.5">
            <label for="scrapPriceInput" class="text-xs font-bold text-slate-700">
              ⑤ アルミ買取相場（キロあたり単価）
            </label>
            <span class="text-[11px] text-slate-500">初期値: 450円/kg</span>
          </div>
          <div class="flex flex-col sm:flex-row items-stretch sm:items-center gap-2">
            <div class="relative flex-1">
              <input type="number" id="scrapPriceInput" step="5" min="50" max="2000" value="450" class="w-full h-12 pl-4 pr-16 text-lg font-bold bg-slate-50 border border-slate-300 rounded-xl focus:bg-white focus:outline-none focus:ring-2 focus:ring-sky-500 font-mono">
              <span class="absolute right-3.5 top-3.5 text-xs text-slate-500 font-semibold pointer-events-none">円 / kg</span>
            </div>
            <div class="flex items-center gap-1.5 overflow-x-auto pb-1 sm:pb-0">
              <button type="button" class="price-preset text-xs px-3 py-2.5 rounded-lg bg-slate-100 hover:bg-slate-200 text-slate-700 font-medium whitespace-nowrap" data-price="400">400円</button>
              <button type="button" class="price-preset text-xs px-3 py-2.5 rounded-lg bg-slate-100 hover:bg-slate-200 text-slate-700 font-medium whitespace-nowrap" data-price="430">430円</button>
              <button type="button" class="price-preset text-xs px-3 py-2.5 rounded-lg bg-sky-100 hover:bg-sky-200 text-sky-800 font-bold whitespace-nowrap" data-price="450">450円 (標準)</button>
              <button type="button" class="price-preset text-xs px-3 py-2.5 rounded-lg bg-slate-100 hover:bg-slate-200 text-slate-700 font-medium whitespace-nowrap" data-price="480">480円</button>
              <button type="button" class="price-preset text-xs px-3 py-2.5 rounded-lg bg-slate-100 hover:bg-slate-200 text-slate-700 font-medium whitespace-nowrap" data-price="500">500円</button>
            </div>
          </div>
          <p class="text-[11px] text-slate-500 mt-1">
            近隣の金属スクラップ店・回収業者の最新買取単価に合わせてご自由に変更してください。
          </p>
        </div>

      </div>

      <!-- 6. （任意）送料の負担 アコーディオン -->
      <div class="border border-slate-200 rounded-xl overflow-hidden bg-slate-50/50">
        <button type="button" id="shippingAccordionBtn" class="w-full px-4 py-3 text-left font-bold text-xs text-slate-700 hover:bg-slate-100 flex items-center justify-between transition">
          <span class="flex items-center gap-2">
            <svg class="w-4 h-4 text-slate-500" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M20 7l-8-4-8 4m16 0l-8 4m8-4v10l-8 4m0-10L4 7m8 4v10M4 7v10l8 4"/></svg>
            <span>（任意）ヤフオク出品時の想定送料負担</span>
            <span id="shippingCurrentBadge" class="text-[10px] font-normal text-slate-500 bg-white px-2 py-0.5 rounded border border-slate-200">
              落札者負担 (¥0)
            </span>
          </span>
          <svg id="shippingChevron" class="w-4 h-4 text-slate-400 transform transition-transform" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M19 9l-7 7-7-7"/></svg>
        </button>

        <div id="shippingAccordionContent" class="hidden px-4 pb-4 pt-1 space-y-3 border-t border-slate-200 bg-white">
          <p class="text-xs text-slate-600 leading-relaxed">
            出品者負担（送料無料出品）にする場合、送料実費を入力してください。落札者負担にする場合は「0円」のままで計算されます。
          </p>
          <div class="flex flex-col sm:flex-row items-stretch sm:items-center gap-2">
            <div class="relative flex-1">
              <input type="number" id="shippingInput" step="100" min="0" max="30000" value="0" placeholder="0" class="w-full h-11 pl-4 pr-12 text-base font-bold bg-slate-50 border border-slate-300 rounded-xl focus:bg-white focus:outline-none focus:ring-2 focus:ring-sky-500 font-mono">
              <span class="absolute right-3.5 top-3 text-xs text-slate-400 font-semibold pointer-events-none">円</span>
            </div>
            <div class="flex items-center gap-1.5 overflow-x-auto">
              <button type="button" class="shipping-preset text-xs px-2.5 py-2 rounded-lg bg-slate-100 hover:bg-slate-200 text-slate-700 whitespace-nowrap font-semibold" data-shipping="0">0円 (落札者負担)</button>
              <button type="button" class="shipping-preset text-xs px-2.5 py-2 rounded-lg bg-slate-100 hover:bg-slate-200 text-slate-700 whitespace-nowrap" data-shipping="3500">3,500円 (2個口目安)</button>
              <button type="button" class="shipping-preset text-xs px-2.5 py-2 rounded-lg bg-slate-100 hover:bg-slate-200 text-slate-700 whitespace-nowrap" data-shipping="6000">6,000円 (4個口目安)</button>
            </div>
          </div>
        </div>
      </div>

    </div>

    <!-- Interactive Simulation & Profit Comparison -->
    <div class="bg-white rounded-2xl p-5 md:p-6 border border-slate-200 shadow-sm space-y-4">
      <div class="flex items-center justify-between">
        <div>
          <h3 class="text-base font-bold text-slate-900 flex items-center gap-2">
            <svg class="w-5 h-5 text-amber-500" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 7h6m0 10v-3m-3 3h.01M9 17h.01M9 14h.01M12 14h.01M15 11h.01M12 11h.01M9 11h.01M7 21h10a2 2 0 002-2V5a2 2 0 00-2-2H7a2 2 0 00-2 2v14a2 2 0 002 2z"/></svg>
            ヤフオク落札予想シミュレーション
          </h3>
          <p class="text-xs text-slate-500 mt-0.5">「もし○○円で落札されたら？」の手取り額とスクラップ買取との差額をチェック</p>
        </div>
      </div>

      <div class="bg-slate-50 p-4 rounded-xl border border-slate-200 space-y-3">
        <div class="flex flex-col sm:flex-row sm:items-center justify-between gap-2">
          <label for="simulationBidInput" class="text-xs font-bold text-slate-700">
            ヤフオク想定落札価格
          </label>
          <div class="flex items-center gap-2">
            <span class="text-xs text-slate-500 font-mono">¥</span>
            <input type="number" id="simulationBidInput" step="1000" min="0" max="500000" value="25000" class="w-36 h-10 px-3 text-right text-base font-bold bg-white border border-slate-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-amber-500 font-mono">
            <span class="text-xs text-slate-500">円</span>
          </div>
        </div>

        <input type="range" id="simulationBidSlider" min="5000" max="80000" step="1000" value="25000" class="w-full accent-amber-500 cursor-pointer">

        <div class="grid grid-cols-1 sm:grid-cols-3 gap-3 pt-2 text-center">
          <div class="bg-white p-3 rounded-lg border border-slate-200">
            <div class="text-[11px] text-slate-400 font-medium">システム手数料 (10%)</div>
            <div id="simFeeDisplay" class="text-sm font-bold text-slate-700 font-mono mt-0.5">-¥2,500</div>
          </div>
          <div class="bg-white p-3 rounded-lg border border-slate-200">
            <div class="text-[11px] text-slate-400 font-medium">ヤフオク手取り額</div>
            <div id="simNetPayoutDisplay" class="text-base font-bold text-slate-900 font-mono mt-0.5">¥22,500</div>
          </div>
          <div id="simDifferenceCard" class="bg-emerald-50 p-3 rounded-lg border border-emerald-200">
            <div id="simDifferenceLabel" class="text-[11px] text-emerald-800 font-bold">ヤフオクの方がお得</div>
            <div id="simDifferenceDisplay" class="text-base font-black text-emerald-600 font-mono mt-0.5">+¥7,200</div>
          </div>
        </div>
      </div>
    </div>

    <!-- Reference Weight Table Accordion -->
    <div class="border border-slate-200 rounded-2xl bg-white overflow-hidden shadow-sm">
      <button type="button" id="tableAccordionBtn" class="w-full px-5 py-4 text-left font-bold text-sm text-slate-800 hover:bg-slate-50 flex items-center justify-between transition">
        <span class="flex items-center gap-2">
          <svg class="w-4 h-4 text-slate-500" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M3 10h18M3 14h18m-9-4v8m-7 0h14a2 2 0 002-2V6a2 2 0 00-2-2H5a2 2 0 00-2 2v8a2 2 0 002 2z"/></svg>
          アルミホイール平均重量目安表（データテーブル）
        </span>
        <svg id="tableChevron" class="w-4 h-4 text-slate-400 transform transition-transform" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M19 9l-7 7-7-7"/></svg>
      </button>

      <div id="tableAccordionContent" class="hidden px-5 pb-5 border-t border-slate-100">
        <div class="overflow-x-auto pt-3">
          <table class="w-full text-xs text-left">
            <thead class="bg-slate-100 text-slate-700 font-bold">
              <tr>
                <th class="p-2.5 rounded-l-lg">インチ</th>
                <th class="p-2.5">純正ホイール (目安)</th>
                <th class="p-2.5">社外ホイール (目安)</th>
                <th class="p-2.5 rounded-r-lg">主な車種・用途</th>
              </tr>
            </thead>
            <tbody id="weightTableBody" class="divide-y divide-slate-100">
              <!-- populated by JS -->
            </tbody>
          </table>
        </div>
        <p class="text-[11px] text-slate-500 mt-3 leading-relaxed">
          ※上記はホイール単体（タイヤ・バランスウェイト・バルブ除く）の代表的な平均重量です。鍛造（RAYS・BBSなど）は上記より軽量、大径SUV向け純正はより重量がある場合があります。
        </p>
      </div>
    </div>

    <!-- Practical Advice Section -->
    <div class="grid grid-cols-1 md:grid-cols-2 gap-4">
      <div class="bg-white rounded-xl p-4 border border-slate-200">
        <h4 class="text-xs font-bold text-slate-900 flex items-center gap-1.5 mb-2">
          <span class="w-2 h-2 rounded-full bg-slate-400"></span>
          金属スクラップ持ち込みのメリット・注意点
        </h4>
        <ul class="text-xs text-slate-600 space-y-1.5 list-disc list-inside">
          <li><strong>傷・ガリ・曲がりがあっても同額で買取</strong>：状態不問で重量換算。</li>
          <li><strong>クレーム・梱包の手間がゼロ</strong>：車に乗せてヤードへ行けば即現金。</li>
          <li><strong>タイヤ付きの注意</strong>：業者によってはタイヤ外し処分料（500〜1,000円/本）が差し引かれる場合があるため、事前に確認を推奨。</li>
        </ul>
      </div>

      <div class="bg-white rounded-xl p-4 border border-slate-200">
        <h4 class="text-xs font-bold text-slate-900 flex items-center gap-1.5 mb-2">
          <span class="w-2 h-2 rounded-full bg-amber-500"></span>
          ヤフオク出品のメリット・注意点
        </h4>
        <ul class="text-xs text-slate-600 space-y-1.5 list-disc list-inside">
          <li><strong>高値がつきやすい銘柄</strong>：人気車種の純正上位グレード、BBS・ENKEI・RAYS・WORK等の社外人気ブランド。</li>
          <li><strong>手間とコストの考慮</strong>：ヤフオク手数料10%、専用ダンボール代（約1,500円）、清掃・梱包（約1〜2時間）、発送手続きが必要です。</li>
          <li>損益分岐点＋5,000円以上で売れる見込みがあれば、出品する価値が十分にあります。</li>
        </ul>
      </div>
    </div>

  </main>

  <!-- Footer -->
  <footer class="bg-slate-900 text-slate-400 text-xs py-6 border-t border-slate-800 mt-8">
    <div class="max-w-4xl mx-auto px-4 flex flex-col sm:flex-row items-center justify-between gap-3 text-center sm:text-left">
      <div>
        <p class="font-medium text-slate-300">アルミホイール売却判断ナビ</p>
        <p class="text-[11px] text-slate-500 mt-0.5">スクラップ持ち込み買取・ヤフオク損益分岐点 自動計算ツール</p>
      </div>
      <div class="text-[11px] text-slate-500">
        計算式: 最低落札額 = (持込買取額 + 送料) ÷ 0.90 (手数料10%)
      </div>
    </div>
  </footer>

  <!-- Application Logic (Vanilla JavaScript) -->
  <script>
    // 1. Weight Table Database
    const WEIGHT_DATA = {
      13: { oem: 6.0, aftermarket: 5.0, note: '軽自動車・軽トラ・コンパクト' },
      14: { oem: 7.0, aftermarket: 6.0, note: '軽ハイトワゴン・コンパクトカー' },
      15: { oem: 8.5, aftermarket: 7.0, note: 'アクア・フィット・プリウス等' },
      16: { oem: 10.0, aftermarket: 8.0, note: 'セレナ・ノア・ヤリスクロス等' },
      17: { oem: 11.5, aftermarket: 9.0, note: 'クラウン・プリウス・RAV4等' },
      18: { oem: 13.0, aftermarket: 10.0, note: 'アルファード・ハリアー・WRX等' },
      19: { oem: 14.5, aftermarket: 11.0, note: 'レクサス・クラウン・大型SUV' },
      20: { oem: 16.0, aftermarket: 12.0, note: 'ランクル・RX・大径カスタム' },
      21: { oem: 17.5, aftermarket: 13.5, note: '超大径SUV・輸入車' },
      22: { oem: 19.0, aftermarket: 14.5, note: '大型SUV・カスタムサイズ' },
    };

    // State Variables
    let currentInch = 15;
    let currentType = 'oem'; // 'oem' | 'aftermarket'
    let currentCount = 4;
    let currentWeight = 8.5;
    let currentScrapPrice = 450;
    let currentShipping = 0;
    let currentSimBid = 25000;
    let isWeightCustomized = false;

    // DOM Elements
    const inchSelect = document.getElementById('inchSelect');
    const typeOEMBtn = document.getElementById('typeOEM');
    const typeAftermarketBtn = document.getElementById('typeAftermarket');
    const countInput = document.getElementById('countInput');
    const countMinusBtn = document.getElementById('countMinus');
    const countPlusBtn = document.getElementById('countPlus');
    const weightInput = document.getElementById('weightInput');
    const weightMinusBtn = document.getElementById('weightMinus');
    const weightPlusBtn = document.getElementById('weightPlus');
    const weightSourceBadge = document.getElementById('weightSourceBadge');
    const scrapPriceInput = document.getElementById('scrapPriceInput');
    const shippingInput = document.getElementById('shippingInput');
    const shippingAccordionBtn = document.getElementById('shippingAccordionBtn');
    const shippingAccordionContent = document.getElementById('shippingAccordionContent');
    const shippingChevron = document.getElementById('shippingChevron');
    const shippingCurrentBadge = document.getElementById('shippingCurrentBadge');
    const shippingNoticeText = document.getElementById('shippingNoticeText');
    const tableAccordionBtn = document.getElementById('tableAccordionBtn');
    const tableAccordionContent = document.getElementById('tableAccordionContent');
    const tableChevron = document.getElementById('tableChevron');
    const weightTableBody = document.getElementById('weightTableBody');
    const resetBtn = document.getElementById('resetBtn');

    // Display Elements
    const scrapTotalPriceDisplay = document.getElementById('scrapTotalPriceDisplay');
    const totalWeightDisplay = document.getElementById('totalWeightDisplay');
    const scrapUnitPriceDisplay = document.getElementById('scrapUnitPriceDisplay');
    const breakEvenPriceDisplay = document.getElementById('breakEvenPriceDisplay');
    const verdictPrice = document.getElementById('verdictPrice');
    const simulationBidInput = document.getElementById('simulationBidInput');
    const simulationBidSlider = document.getElementById('simulationBidSlider');
    const simFeeDisplay = document.getElementById('simFeeDisplay');
    const simNetPayoutDisplay = document.getElementById('simNetPayoutDisplay');
    const simDifferenceCard = document.getElementById('simDifferenceCard');
    const simDifferenceLabel = document.getElementById('simDifferenceLabel');
    const simDifferenceDisplay = document.getElementById('simDifferenceDisplay');

    // Populate Data Table
    function renderTable() {
      weightTableBody.innerHTML = Object.entries(WEIGHT_DATA).map(([inch, data]) => `
        <tr class="hover:bg-slate-50 transition ${inch == currentInch ? 'bg-sky-50/70 font-semibold text-sky-950' : ''}">
          <td class="p-2.5 font-bold">${inch}インチ</td>
          <td class="p-2.5 font-mono ${currentType === 'oem' && inch == currentInch ? 'text-sky-700 font-black' : ''}">約 ${data.oem.toFixed(1)} kg</td>
          <td class="p-2.5 font-mono ${currentType === 'aftermarket' && inch == currentInch ? 'text-sky-700 font-black' : ''}">約 ${data.aftermarket.toFixed(1)} kg</td>
          <td class="p-2.5 text-slate-500">${data.note}</td>
        </tr>
      `).join('');
    }

    // Helper: Format Number with Commas
    function formatYen(num) {
      return Math.round(num).toLocaleString('ja-JP');
    }

    // Auto-update weight from standard table
    function syncDefaultWeight() {
      const data = WEIGHT_DATA[currentInch] || { oem: 10.0, aftermarket: 8.0 };
      currentWeight = currentType === 'oem' ? data.oem : data.aftermarket;
      weightInput.value = currentWeight.toFixed(1);
      isWeightCustomized = false;
      weightSourceBadge.textContent = '目安テーブル適用中';
      weightSourceBadge.className = 'text-[10px] text-sky-700 bg-sky-50 px-1.5 py-0.5 rounded border border-sky-200';
    }

    // Main Calculation & UI Update
    function calculate() {
      // Inputs Sanitization
      const weight = Math.max(0.1, parseFloat(weightInput.value) || 0);
      const count = Math.max(1, parseInt(countInput.value, 10) || 1);
      const scrapUnitPrice = Math.max(0, parseFloat(scrapPriceInput.value) || 0);
      const shipping = Math.max(0, parseFloat(shippingInput.value) || 0);

      currentWeight = weight;
      currentCount = count;
      currentScrapPrice = scrapUnitPrice;
      currentShipping = shipping;

      // 1. Scrap metal calculation
      const totalWeight = Number((weight * count).toFixed(2));
      const scrapTotalPrice = Math.round(totalWeight * scrapUnitPrice);

      // 2. Yahoo Auction Break-Even calculation
      // Net Payout = Bid * 0.90 - Shipping
      // Break-Even Bid = (Scrap Total Price + Shipping) / 0.90
      const breakEvenRaw = (scrapTotalPrice + shipping) / 0.90;
      // Round up to nearest 10 yen for realistic auction bidding
      const breakEvenPrice = Math.ceil(breakEvenRaw / 10) * 10;

      // Update Displays
      scrapTotalPriceDisplay.textContent = formatYen(scrapTotalPrice);
      totalWeightDisplay.textContent = totalWeight.toFixed(1);
      scrapUnitPriceDisplay.textContent = formatYen(scrapUnitPrice);
      breakEvenPriceDisplay.textContent = formatYen(breakEvenPrice);
      verdictPrice.textContent = '¥' + formatYen(breakEvenPrice);

      if (shipping > 0) {
        shippingNoticeText.textContent = ` + 送料¥${formatYen(shipping)}`;
        shippingCurrentBadge.textContent = `出品者負担 (¥${formatYen(shipping)})`;
        shippingCurrentBadge.className = 'text-[10px] font-bold text-amber-700 bg-amber-50 px-2 py-0.5 rounded border border-amber-200';
      } else {
        shippingNoticeText.textContent = '';
        shippingCurrentBadge.textContent = '落札者負担 (¥0)';
        shippingCurrentBadge.className = 'text-[10px] font-normal text-slate-500 bg-white px-2 py-0.5 rounded border border-slate-200';
      }

      // Update Simulation
      updateSimulation(scrapTotalPrice, shipping);

      // Update table highlight
      renderTable();
    }

    // Simulation calculation
    function updateSimulation(scrapTotalPrice, shipping) {
      const bid = Math.max(0, parseFloat(simulationBidInput.value) || 0);
      const fee = Math.round(bid * 0.10);
      const netPayout = Math.round(bid - fee - shipping);
      const diff = netPayout - scrapTotalPrice;

      simFeeDisplay.textContent = `-¥${formatYen(fee)}`;
      simNetPayoutDisplay.textContent = `¥${formatYen(netPayout)}`;

      if (diff >= 0) {
        simDifferenceCard.className = 'bg-emerald-50 p-3 rounded-lg border border-emerald-200';
        simDifferenceLabel.textContent = 'ヤフオクの方がお得';
        simDifferenceLabel.className = 'text-[11px] text-emerald-800 font-bold';
        simDifferenceDisplay.textContent = `+¥${formatYen(diff)}`;
        simDifferenceDisplay.className = 'text-base font-black text-emerald-600 font-mono mt-0.5';
      } else {
        simDifferenceCard.className = 'bg-rose-50 p-3 rounded-lg border border-rose-200';
        simDifferenceLabel.textContent = '持ち込み買取の方がお得';
        simDifferenceLabel.className = 'text-[11px] text-rose-800 font-bold';
        simDifferenceDisplay.textContent = `持ち込み +${formatYen(Math.abs(diff))}円`;
        simDifferenceDisplay.className = 'text-base font-black text-rose-600 font-mono mt-0.5';
      }
    }

    // Event Listeners

    // Inch change
    inchSelect.addEventListener('change', (e) => {
      currentInch = parseInt(e.target.value, 10);
      syncDefaultWeight();
      calculate();
    });

    // Type selection
    function setType(type) {
      currentType = type;
      if (type === 'oem') {
        typeOEMBtn.className = 'type-btn rounded-lg text-sm font-bold transition flex items-center justify-center gap-1.5 bg-white text-slate-900 shadow-sm border border-slate-200';
        typeOEMBtn.querySelector('svg').className = 'w-4 h-4 text-sky-600';
        typeAftermarketBtn.className = 'type-btn rounded-lg text-sm font-semibold transition flex items-center justify-center gap-1.5 text-slate-600 hover:text-slate-900';
        typeAftermarketBtn.querySelector('svg').className = 'w-4 h-4 text-slate-400';
      } else {
        typeAftermarketBtn.className = 'type-btn rounded-lg text-sm font-bold transition flex items-center justify-center gap-1.5 bg-white text-slate-900 shadow-sm border border-slate-200';
        typeAftermarketBtn.querySelector('svg').className = 'w-4 h-4 text-sky-600';
        typeOEMBtn.className = 'type-btn rounded-lg text-sm font-semibold transition flex items-center justify-center gap-1.5 text-slate-600 hover:text-slate-900';
        typeOEMBtn.querySelector('svg').className = 'w-4 h-4 text-slate-400';
      }
      syncDefaultWeight();
      calculate();
    }
    typeOEMBtn.addEventListener('click', () => setType('oem'));
    typeAftermarketBtn.addEventListener('click', () => setType('aftermarket'));

    // Count controls
    countMinusBtn.addEventListener('click', () => {
      const val = Math.max(1, (parseInt(countInput.value, 10) || 1) - 1);
      countInput.value = val;
      calculate();
    });
    countPlusBtn.addEventListener('click', () => {
      const val = Math.min(20, (parseInt(countInput.value, 10) || 1) + 1);
      countInput.value = val;
      calculate();
    });
    countInput.addEventListener('input', calculate);

    document.querySelectorAll('.count-preset').forEach(btn => {
      btn.addEventListener('click', () => {
        countInput.value = btn.dataset.count;
        calculate();
      });
    });

    // Weight controls
    weightInput.addEventListener('input', () => {
      isWeightCustomized = true;
      weightSourceBadge.textContent = '手動入力中';
      weightSourceBadge.className = 'text-[10px] text-amber-800 bg-amber-50 px-1.5 py-0.5 rounded border border-amber-200';
      calculate();
    });
    weightMinusBtn.addEventListener('click', () => {
      const val = Math.max(0.5, (parseFloat(weightInput.value) || 0) - 0.5);
      weightInput.value = val.toFixed(1);
      isWeightCustomized = true;
      weightSourceBadge.textContent = '手動入力中';
      weightSourceBadge.className = 'text-[10px] text-amber-800 bg-amber-50 px-1.5 py-0.5 rounded border border-amber-200';
      calculate();
    });
    weightPlusBtn.addEventListener('click', () => {
      const val = Math.min(40, (parseFloat(weightInput.value) || 0) + 0.5);
      weightInput.value = val.toFixed(1);
      isWeightCustomized = true;
      weightSourceBadge.textContent = '手動入力中';
      weightSourceBadge.className = 'text-[10px] text-amber-800 bg-amber-50 px-1.5 py-0.5 rounded border border-amber-200';
      calculate();
    });

    // Scrap price controls
    scrapPriceInput.addEventListener('input', calculate);
    document.querySelectorAll('.price-preset').forEach(btn => {
      btn.addEventListener('click', () => {
        scrapPriceInput.value = btn.dataset.price;
        calculate();
      });
    });

    // Shipping controls
    shippingInput.addEventListener('input', calculate);
    document.querySelectorAll('.shipping-preset').forEach(btn => {
      btn.addEventListener('click', () => {
        shippingInput.value = btn.dataset.shipping;
        calculate();
      });
    });

    // Accordions
    shippingAccordionBtn.addEventListener('click', () => {
      const isHidden = shippingAccordionContent.classList.contains('hidden');
      if (isHidden) {
        shippingAccordionContent.classList.remove('hidden');
        shippingChevron.classList.add('rotate-180');
      } else {
        shippingAccordionContent.classList.add('hidden');
        shippingChevron.classList.remove('rotate-180');
      }
    });

    tableAccordionBtn.addEventListener('click', () => {
      const isHidden = tableAccordionContent.classList.contains('hidden');
      if (isHidden) {
        tableAccordionContent.classList.remove('hidden');
        tableChevron.classList.add('rotate-180');
      } else {
        tableAccordionContent.classList.add('hidden');
        tableChevron.classList.remove('rotate-180');
      }
    });

    // Simulation controls
    simulationBidInput.addEventListener('input', (e) => {
      const val = parseFloat(e.target.value) || 0;
      simulationBidSlider.value = val;
      calculate();
    });
    simulationBidSlider.addEventListener('input', (e) => {
      simulationBidInput.value = e.target.value;
      calculate();
    });

    // Reset button
    resetBtn.addEventListener('click', () => {
      currentInch = 15;
      inchSelect.value = '15';
      setType('oem');
      countInput.value = '4';
      scrapPriceInput.value = '450';
      shippingInput.value = '0';
      simulationBidInput.value = '25000';
      simulationBidSlider.value = '25000';
      syncDefaultWeight();
      calculate();
    });

    // Initial run
    syncDefaultWeight();
    renderTable();
    calculate();
  </script>
</body>
</html>
