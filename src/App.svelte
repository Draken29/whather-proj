<script>
  import './app.css';
  import { onMount } from "svelte";
  
  
  let dailyDates = [];
  let dailyMax = [];
  let dailyMin = [];
  let dailyWeatherCode = [];
  let selectedDay = 0; 

  onMount(async () => {
    const url =
      "https://api.open-meteo.com/v1/forecast?latitude=33.3152&longitude=44.3661&daily=weather_code,temperature_2m_max,temperature_2m_min&timezone=auto";

    const response = await fetch(url);
    const data = await response.json();

    dailyDates = data.daily.time;
    dailyMax = data.daily.temperature_2m_max;
    dailyMin = data.daily.temperature_2m_min;
    dailyWeatherCode = data.daily.weather_code;
  });

  function getWeatherText(code) {
    if (code === 0) return "صافي ☀️";
    if (code === 1 || code === 2) return "غائم جزئيًا 🌤️";
    if (code === 3) return "غائم ☁️";
    if (code >= 51 && code <= 67) return "مطر 🌧️";
    if (code >= 71 && code <= 77) return "ثلج ❄️";
    if (code >= 95) return "عواصف 🌩️";
    return "غير معروف";
  }

  function showDay(offset) {
    selectedDay = offset;
  }
</script>


<main class="bg-[url('w1.jpg')] bg-no-repeat bg-center bg-cover h-screen text-center text-white p-6">
  <br><br><br><br><br><br>
<div class="bg-black/30 p-6 rounded-lg inline-block h-auto">
  <div class=" items-center gap-4 bg-black/35  rounded-lg">
    <br>
    <h2 class="text-3xl ">{getWeatherText(dailyWeatherCode[1 + selectedDay])}</h2>
    
    <p class="text-m relative left-[-5%] mt-4">
      {dailyDates[1 + selectedDay]}
    </p>
    <br>
  </div>
    <br>
    <p class="text-2xl">
      H.T: {dailyMax[1 + selectedDay]}°C
    </p>
    <p class="text-2xl">
      M.T: {dailyMin[1 + selectedDay]}°C
    </p>
   
  

  <h3 class="text-xl mt-10">اختر اليوم</h3>

  <div class="flex justify-center gap-4 mt-4">
    <button on:click={() => showDay(3)} class="bg-white/20 px-4 py-2 rounded-lg hover:bg-white/30">
      2
    </button>
    <button on:click={() => showDay(2)} class="bg-white/20 px-4 py-2 rounded-lg hover:bg-white/30">
      1
    </button>
    <button on:click={() => showDay(1)} class="bg-white/20 px-4 py-2 rounded-lg hover:bg-white/30">
      30
    </button>
    <button on:click={() => showDay(0)} class="bg-white/20 px-4 py-2 rounded-lg hover:bg-white/30">
      29
    </button>
    <button on:click={() => showDay(-1)} class="bg-white/20 px-4 py-2 rounded-lg hover:bg-white/30">
      28
    </button>
    
  </div>
</div>
</main>
