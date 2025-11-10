<script>
  import { onMount } from 'svelte';

  let activeSection = 'zero-rated';
  let expandedTroubleshooting = null;

  // Új: szerver típus választó (v2ray vagy ssh)
  let serverType = 'v2ray';

  const sections = [
    { id: 'zero-rated', label: 'Zero Rated oldal keresése', icon: '1' },
    { id: 'vmess-server', label: 'V2Ray vagy SSH szerver beszerzése', icon: '2' },
    { id: 'apps', label: 'Szükséges alkalmazások', icon: '3' },
    { id: 'settings', label: 'Beállítások', icon: '4' },
    { id: 'connection', label: 'Csatlakozás', icon: '5' },
    { id: 'hotspot', label: 'Hotspot megosztás', icon: '6' },
    { id: 'troubleshooting', label: 'Hibaelhárítás', icon: '7' }
  ];

  function toggleTroubleshooting(id) {
    expandedTroubleshooting = expandedTroubleshooting === id ? null : id;
  }

  function scrollToSection(id) {
    activeSection = id;
    const el = document.getElementById(id);
    if (el) el.scrollIntoView({ behavior: 'smooth', block: 'start' });
  }

  onMount(() => {
    document.documentElement.classList.add('dark');
  });
</script>

<main class="flex items-center justify-center min-h-screen bg-gray-900 text-white">
  <div class="max-w-2xl p-8 w-full">
    <a href="../" class="inline-block mb-6 text-blue-400 hover:underline">← Vissza a főoldalra</a>
    <h1 class="text-4xl font-bold mb-6">Ingyen mobilnet Skellytől</h1>

    <!-- Tartalomjegyzék -->
    <div class="bg-gray-800 rounded-lg p-4 shadow-md mb-8">
      <h2 class="text-2xl font-semibold mb-3 text-white">Tartalomjegyzék (kattintható)</h2>
      <ul class="space-y-2">
        {#each sections as section}
          <li>
            <button on:click={() => scrollToSection(section.id)} class="text-blue-400 hover:underline flex items-center w-full">
              <span class="mr-2">{section.icon}.</span> {section.label}
            </button>
          </li>
        {/each}
      </ul>
    </div>

    <!-- Fontos tudnivalók -->
    <div class="bg-red-900/30 border-l-4 border-red-800 p-3 rounded-r-md mt-3 mb-8">
      <p class="text-red-400">
        ⚠️ <strong>Fontos tudnivalók:</strong>
      </p>
      <ul class="list-disc list-inside space-y-2 mt-2 ml-2">
        <li>Kevés, de <strong>megvan az esély hogy lekorlátozzák vagy letiltják a SIM kártyádat</strong></li>
        <li>Csak <strong>saját felelősségre</strong> használd, vagy olyan SIM kártyával amit nem sajnálsz</li>
        <li>A leírtak <strong>kizárólag oktatási célból</strong> lettek megosztva</li>
        <li>Az alábbiakban leírt módszer nagy valószínűséggel a SIM szolgáltatód <strong>ÁSZF-e ellen van</strong></li>
        <li>Ha a telefonod támogatja a <strong>Dual SIM-et</strong>, akkor tudod a fő sim kártyád mellett ezt használni</li>
        <li>A SIM aktiválásához <strong>minimum 14 éves életkor szükséges, illetve csak feltöltő kártyás SIM-mel működik</strong></li>
      </ul>
    </div>

    <!-- Zero Rated -->
    <div id="zero-rated" class="bg-gray-800 rounded-lg p-4 mb-6 shadow-md scroll-mt-20">
      <h2 class="text-2xl font-semibold mb-3 text-blue-400">Első lépés: Zero Rated oldal keresése</h2>
      <p>
        Először is mielőtt elkezdenéd, kell keresned egy <span class="font-semibold text-blue-300">"zero rated" oldalt</span>.
        Ez azt jelenti hogy a sim szolgáltatód megengedi hogy felmenj egy megadott oldalra/oldalakra mobilnet nélkül is.
      </p>
      <p>A szolgáltatók általában megengedik, hogy a saját oldalukra felmenj úgy is ha elfogyott a mobilneted. Például:</p>
      <div class="bg-gray-700 p-3 rounded-md my-3">
        <ul class="list-disc list-inside pl-2 space-y-1">
          <li><strong>Yettel:</strong> <code class="bg-gray-600 p-1 rounded">yettel.hu</code>, <code class="bg-gray-600 p-1 rounded">speedtest.net</code>, <code class="bg-gray-600 p-1 rounded">fast.com</code></li>
          <li><strong>Telekom és One:</strong> Sajnos nem működik</li>
        </ul>
      </div>
      <div class="bg-yellow-900/30 border-l-4 border-yellow-500 p-3 rounded-r-md mt-3">
        <p class="text-yellow-300">
          ⚠️ Ez előfizetéses kártya illetve Yettel Yepp nem fog menni tehát mindenképp kell egy feltöltőkártyás SIM.
          Ne aggódj, nem kell feltöltened rá pénzt csak szimplán aktiválnod kell és ha lejár akkor veszel egy új kártyát. Van esély, hogy feltöltő kártyával se működik,
          ez személyfüggő.
        </p>
      </div>
    </div>

    <!-- Szerver beszerzése -->
    <div id="vmess-server" class="bg-gray-800 rounded-lg p-4 mb-6 shadow-md scroll-mt-20">
      <h2 class="text-2xl font-semibold mb-3 text-green-400">Szerver beszerzése (kattintható gombok)</h2>

      <div class="flex gap-2 mb-4">
        <button 
          class="{serverType === 'v2ray' ? 'bg-green-600 text-white' : 'bg-gray-700 text-gray-300 hover:bg-gray-600'} px-4 py-2 rounded font-semibold"
          on:click={() => serverType = 'v2ray'}
        >V2Ray VMESS</button>
        <button 
          class="{serverType === 'ssh' ? 'bg-green-600 text-white' : 'bg-gray-700 text-gray-300 hover:bg-gray-600'} px-4 py-2 rounded font-semibold"
          on:click={() => serverType = 'ssh'}
        >SSH Szerver</button>
      </div>

      {#if serverType === 'v2ray'}
        <p>Ha sikerült találnod egy zero rated oldalt, akkor szerezz egy V2Ray VMESS szervert.</p>
        <ol class="list-disc list-inside space-y-2 pl-2 my-3">
          <p>Ezt akár saját szervereddel is bírod hosztolni</p>
          <li>Az egyszerűség kedvéért itt vannak oldalak, ahol ingyen tudsz szervert generálni:</li>
          <ul class="list-disc list-inside pl-6 text-blue-400 mt-1">
            <li><a href="https://sshs8.com/" class="text-blue-400 underline">sshs8.com</a></li>
            <li><a href="https://www.vpnjantit.com/free-v2ray-vmess" class="text-blue-400 underline">vpnjantit.com</a> (itt tudsz venni olcsón szervert, ez persze nem kötelező)</li>
          </ul>
          <li>Válassz lehetőleg egy közel lévő szervert (pl. Németország)</li>
          <li>Írj be egy random jelszót (ha kér), majd kattints a "Create an Account"-ra</li>
          <li>Ha van link amit ki tudsz másolni, akkor azzal másold át az alkalmazásba, ha nincs akkor manuálisan csináld meg</li>
        </ol>
        <div class="bg-blue-900/30 border-l-4 border-blue-500 p-3 rounded-r-md mt-3">
          <p class="text-blue-300">
            💡 Ez egy komplett link, amivel nem kell egyesével kimásolnod az adatokat, csak beilleszted az appba.
          </p>
        </div>
      {:else}
        <p>Az SSH egy másik fajta szerver, amire HTTP Injector appal rá tudsz csatlakozni. Ez lehet hogy biztonságosabb bár itt meglehet hogy nem megy minden alkalmazás/oldal vele.</p>
        <ul class="list-disc list-inside space-y-2 pl-6 my-3">
        </ul>
        <p>Szintén ugyan ezen az oldalakon tudsz SSH szervert is generálni:</p>
        <ul class="list-disc list-inside pl-6 my-2">
          <li><a href="https://sshs8.com/" class="text-blue-400 underline">sshs8.com</a></li>
          <li><a href="https://www.vpnjantit.com" class="text-blue-400 underline">vpnjantit.com</a></li>
        </ul>
        <div class="bg-green-900/30 border-l-4 border-green-500 p-3 rounded-r-md">
          <p class="text-green-300">
            💡 Az SSH szervernél szükséged lesz: <code>host</code>, <code>port</code>, <code>felhasználónév</code>, <code>jelszó</code>.
            Ezeket a legtöbb generátor megadja automatikusan.
          </p>
        </div>
      {/if}
    </div>

    <!-- Alkalmazások -->
    <div id="apps" class="bg-gray-800 rounded-lg p-4 mb-6 shadow-md scroll-mt-20">
      <h2 class="text-2xl font-semibold mb-3 text-purple-400">Szükséges alkalmazások</h2>
      <div class="flex flex-col md:flex-row md:space-x-4 space-y-3 md:space-y-0 my-3">
        <div class="flex-1 bg-gray-700 p-3 rounded">
          <h3 class="font-semibold text-lg mb-2">Android (V2Ray)</h3>
          <a href="https://play.google.com/store/apps/details?id=com.github.v2raygg" class="text-blue-400 underline">v2rayGG</a>
        </div>
        <div class="flex-1 bg-gray-700 p-3 rounded">
          <h3 class="font-semibold text-lg mb-2">iOS (V2Ray+SSH)</h3>
          <a href="https://apps.apple.com/us/app/npv-tunnel/id1629465476" class="text-blue-400 underline">Npv Tunnel</a>
        </div>
      </div>
      <div class="flex flex-col md:flex-row md:space-x-4 space-y-3 md:space-y-0 my-3">
        <div class="flex-1 bg-gray-700 p-3 rounded">
          <h3 class="font-semibold text-lg mb-2">Android (SSH)</h3>
          <a href="https://play.google.com/store/apps/details?id=com.evozi.injector" class="text-blue-400 underline">HTTP Injector</a>
        </div>
        <div class="flex-1 bg-gray-700 p-3 rounded">
          <h3 class="font-semibold text-lg mb-2">iOS (SSH)</h3>
          <a href="https://apps.apple.com/us/app/http-injector/id1659992827" class="text-blue-400 underline">HTTP Injector</a>
        </div>
      </div>
      <div class="flex flex-col md:flex-row md:space-x-4 space-y-3 md:space-y-0 my-3">
      </div>
      <div class="bg-yellow-900/30 border-l-4 border-yellow-500 p-3 rounded-r-md mt-4">

      </div>
    </div>

    <!-- Beállítások -->
    <div id="settings" class="bg-gray-800 rounded-lg p-4 mb-6 shadow-md scroll-mt-20">
      <h2 class="text-2xl font-semibold mb-3 text-yellow-400">Beállítások</h2>
      <div class="mb-4">
        <h3 class="font-semibold text-lg mb-2">V2Ray alapbeállítás</h3>
        <ol class="list-decimal list-inside space-y-2 pl-2">
          <li><strong>Android:</strong> Nyomd meg a "+" gombot → "Import config from clipboard"</li>
          <li><strong>iOS:</strong> Nyomd meg a "+" gombot → "Import v2ray URI from clipboard"</li>
          <li>Ha megjelent az új szerver a listán, nyomj rá a szerkesztés gombra</li>
          <li>Keresd meg a <code class="bg-gray-600 p-1 rounded">ws host</code> mezőt (iOS-en: <code class="bg-gray-600 p-1 rounded">Request Host</code>)</li>
          <li>Ide írd be a zero rated oldalt, pl. <code class="bg-gray-600 p-1 rounded">www.yettel.hu</code></li>
        </ol>
      </div>

      <div class="mb-4 border-t border-gray-700 pt-4">
        <h3 class="font-semibold text-lg mb-2">SSH + TLS/SSL Stunnel beállítás HTTP Injectorban</h3>
        <ol class="list-decimal list-inside space-y-2 pl-2">
          <li>Nyisd meg a <strong>HTTP Injector</strong> appot</li>
          <li>Menj az <strong>Settings</strong> → <strong>SSH</strong> részhez</li>
          <li>Add meg az SSH szerver adatait:
            <ul class="list-disc list-inside pl-6 mt-1 space-y-1">
              <li><code class="bg-gray-600 p-1 rounded">Host</code>: Itt lehetőleg az IP verzióját másold be (pl. 51.69.195.252), a vpnjantit oldalon a host alatt van egy gomb amivel meg tudod nézni az IP cím verzióját</li>
              <li><code class="bg-gray-600 p-1 rounded">Port</code>: SSH port (általában 22 vagy 443), ssh8.com oldalról generált szerveren használd a 445 portot, ennél megy az UDP és TCP is</li>
              <li><code class="bg-gray-600 p-1 rounded">Username</code>: felhasználónév</li>
              <li><code class="bg-gray-600 p-1 rounded">Password</code>: jelszó</li>
            </ul>
          </li>
          <li>Menj vissza a főoldalra</li>
          <li>Nyomj rá a start alatti gombra és válaszd ki SSH és TLS/SSL (Stunnel), majd nyomj a mentés gombra</li>
          <li>Majd az ez alatti mezőbe ird be az egyik Zero Rated oldalt, mint például "www.yettel.hu"</li>
        </ol>
      </div>


    <!-- Csatlakozás -->
    <div id="connection" class="bg-gray-800 rounded-lg p-4 mb-6 shadow-md scroll-mt-20">
      <h2 class="text-2xl font-semibold mb-3 text-blue-400">Csatlakozás</h2>
      <p>Már csak annyit kell csinálnod, hogy rácsatlakozol a szerverre mobilnetről:</p>
      <div class="flex flex-col md:flex-row md:space-x-4 space-y-3 md:space-y-0 my-3">
        <div class="flex-1 bg-gray-700 p-3 rounded">
          <h3 class="font-semibold text-lg mb-2">Android</h3>
          <p>Válaszd ki a szervert, majd nyomd meg a <strong>Start</strong> gombot</p>
        </div>
        <div class="flex-1 bg-gray-700 p-3 rounded">
          <h3 class="font-semibold text-lg mb-2">iOS</h3>
          <p>Válaszd ki a szervert, majd húzd jobbra a <strong>Slide to Connect</strong> csúszkát</p>
        </div>
      </div>
      <div class="bg-blue-900/30 border-l-4 border-blue-500 p-3 rounded-r-md mt-4">
        <p class="text-blue-300">
          💡 Az SSH + Stunnel kapcsolat valamelyest lassabb lehet, de kevesebb az esély hogy az internet/sim szolgáltatód tiltja.
        </p>
      </div>
    </div>

    <!-- Hotspot megosztás -->
    <div id="hotspot" class="bg-gray-800 rounded-lg p-4 mb-6 shadow-md scroll-mt-20">
      <h2 class="text-2xl font-semibold mb-3 text-green-400">Hotspot megosztás</h2>
      <div class="mb-4">
        <h3 class="font-semibold text-lg mb-2 text-yellow-300">Megosztás csak böngészéshez (TCP)</h3>
        <p>Ha úgy akarsz netet osztani, hogy a másik eszközre ne kelljen appot letölteni, akkor a hoszt telefonról a <strong>HTTP Injector</strong> appot kell használnod és azon keresztül proxyval osztani hotspotot.</p>
        <div class="bg-gray-700 p-3 rounded my-3">
          <p class="text-yellow-300 mb-2">⚠️ Korlátozások:</p>
          <p>Ilyenkor csak a TCP fog menni és az UDP nem, így például böngésző működik, de online játék nem.</p>
          <div class="mt-3 border-t border-gray-600 pt-2">
            <ul class="list-disc list-inside pl-2 text-sm">
              <li><strong>TCP:</strong> Általában böngészésre vagy olyan helyen használják ahol fontos hogy az adatok épségben érkezzenek. Ezért ez persze lassabb lesz.</li>
              <li><strong>UDP:</strong> Leginkább játékoknál használják, ez képes akár valósidejű csatlakozásokra, viszont ez nem garantálja az adatok sorrendjét vagy épségét.</li>
            </ul>
          </div>
        </div>
      </div>

      <div class="mb-4">
        <h3 class="font-semibold text-lg mb-2 text-yellow-300">Teljes megosztás (TCP + UDP)</h3>
        <p>Ha azt szeretnéd, hogy a másik eszközön is menjen az UDP és TCP forgalom:</p>
        <ol class="list-decimal list-inside space-y-2 pl-2">
          <li>Telepíts egy hasonló appot a másik eszközre is</li>
          <li>Csatlakozz másik (v2ray vagy ssh) szerverhez az appon keresztül, mint a hoszt</li>
          <li>Mindkét eszközön kapcsolódj szerverre</li>
        </ol>
      </div>

      <div class="bg-gray-800 border border-gray-700 rounded-lg p-4 mb-4">
        <h3 class="font-semibold text-lg mb-3 text-blue-400">HTTP Injector részletes beállítása:</h3>
        <ol class="list-decimal list-inside space-y-2 pl-2">
          <li>Töltsd le a HTTP Injector appot mindkét eszközre (Android/iOS)</li>
          <li>Menj a Beállítások → V2Ray/Xray vagy SSH + Stunnel protokollba</li>
          <li>Felül nyomj a + ikonra, és másold be a szerver linkjét vagy adatait</li>
          <li>V2Ray esetén a Core legyen V2Ray-re állítva (ne Xray)</li>
          <li>Mentés után a Tools menübe lesz egy Hotshare opció (csak androidon)</li>
        </ol>
        <div class="bg-blue-900/30 border-l-4 border-blue-500 p-3 rounded-r-md mt-3 mb-4">
          <p class="text-blue-300">
            💡 A Hotshare megfelel mivel a legtöbb embernek nincs rootolva a telefonja
          </p>
        </div>
        <p>Az app mutatni fogja mit kell csinálnod (EZ CSAK ANDROID HOTSPOT HOSZTON)</p>
        <ol class="list-decimal list-inside space-y-2 pl-2">
          <li>Állítsd be a hotspot jelszavát</li>
          <li>Nyomd meg a "Start Wi-Fi Hotspot" gombot</li>
          <li>Pipáld be a "HTTP" opciót</li>
          <li>Nyomd meg a "START HOTSHARE" gombot</li>
        </ol>
        <div class="bg-gray-700 p-3 rounded my-3">
          <h4 class="font-semibold mb-2 underline">A másik eszközön:</h4>
          <ol class="list-decimal list-inside space-y-2 pl-2">
            <li>Csatlakozz a megosztott hotspothoz</li>
            <li>Állíts proxy-t manuálisan (Android/iOS wifi beállításoknál)</li>
            <li>Proxy beállítások:
              <ul class="list-disc list-inside pl-4 space-y-1 mt-1">
                <li>Szerver: <code>192.168.0.54</code> (vagy amit a host mutat)</li>
                <li>Port: <code>44355</code> (vagy amit a host mutat)</li>
                <li>A többi mezőt hagyd üresen</li>
              </ul>
            </li>
            <li>Mentsd el, majd csatlakozz újra a hálózatra</li>
          </ol>
        </div>
      </div>
    </div>

    <!-- Hibaelhárítás -->
    <div id="troubleshooting" class="bg-gray-800 rounded-lg p-4 mb-6 shadow-md scroll-mt-20">
      <h2 class="text-2xl font-semibold mb-3 text-red-400">Hibaelhárítás (kattintható gombok)</h2>
      <div>
        <div class="bg-gray-700 p-3 rounded cursor-pointer hover:bg-gray-650" on:click={() => toggleTroubleshooting('tls')}>
          <p class="font-semibold flex justify-between items-center">
            <span>TLS/SSL probléma</span>
            <span>{expandedTroubleshooting === 'tls' ? '▼' : '▶'}</span>
          </p>
          {#if expandedTroubleshooting === 'tls'}
          <div class="mt-3 border-t border-gray-600 pt-3 text-sm">
            <p>Lesznek olyan oldalak, amik nem adnak "No TLS" linket, tehát amikor beilleszted a linket és szerkeszted, akkor alul a TLS be lesz nyomva.</p>
            <p class="font-semibold mt-2">Megoldás V2Ray esetén:</p>
            <ol class="list-decimal list-inside pl-2">
              <li>Keresd meg az <code>allowInsecure</code> opciót</li>
              <li>Állítsd át <code>true</code>-ra</li>
            </ol>
          </div>
          {/if}
        </div>
        <div class="bg-gray-700 p-3 rounded cursor-pointer hover:bg-gray-650 mt-3" on:click={() => toggleTroubleshooting('noconnection')}>
          <p class="font-semibold flex justify-between items-center">
            <span>Nem megy a net</span>
            <span>{expandedTroubleshooting === 'noconnection' ? '▼' : '▶'}</span>
          </p>
          {#if expandedTroubleshooting === 'noconnection'}
          <div class="mt-3 border-t border-gray-600 pt-3 text-sm">
            <p>Ha nem tud csatlakozni a szerverhez, próbáld meg:</p>
            <ol class="list-decimal list-inside pl-2">
              <li>Be majd ki kapcsolni a repülőgépes üzemmódot</li>
              <li>Újraindítani az appot</li>
              <li>Másik szervert kipróbálni</li>
							<li>Ellenőrizd hogy biztosan jó SIMet választottál ki mobilnethez</li>
            </ol>
          </div>
						<li>Az ingyenes szerver generátorok általába csak 1 hetes szervereket adnak, tehát 1 hét útán kell újat generálj. Vpnjantit-nál tudsz venni szervert akár 1 évre, viszont nem biztos hogy mindig stabilak lesznek a szerverei.</li>
          {/if}
        </div>
        <div class="bg-gray-700 p-3 rounded cursor-pointer hover:bg-gray-650 mt-3" on:click={() => toggleTroubleshooting('partialfail')}>
          <p class="font-semibold flex justify-between items-center">
            <span>Nem működő oldalak/alkalmazások</span>
            <span>{expandedTroubleshooting === 'partialfail' ? '▼' : '▶'}</span>
          </p>
          {#if expandedTroubleshooting === 'partialfail'}
          <div class="mt-3 border-t border-gray-600 pt-3 text-sm">
            <p>Egy szerveren keresztül csatlakozol a netre ezért ez olyan, mint egy VPN. Amilyen szervert generáltál az oldalon olyan IP címed lesz. Példáúl ha az e-krétára próbálsz belépni német IP címmel akkor ez nem fog menni mivel a kréta tiltja a külföldi címeket</p>

						


          </div>
          {/if}
        </div>
      </div>
    </div>

  </div>
</main>

<style>
  :global(body) {
    @apply bg-gray-900;
  }
  :global(html.dark) {
    color-scheme: dark;
  }
  :global(html) {
    scroll-behavior: smooth;
  }
  :global(code) {
    @apply font-mono text-sm;
  }
</style>
