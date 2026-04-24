<template>
  <main class="page">
    <section class="element-one">
      <button class="lang-switch" type="button" @click="toggleLocale">
        中/EN
      </button>

      <header class="page-head">
        <h1>{{ text.title }}</h1>
        <p>{{ text.subtitle }}</p>
        <button class="quick-download" type="button" @click="downloadByDevice">
          {{ text.quickDownload }}
        </button>
      </header>

      <div class="element-two-grid">
        <article class="element-two">
          <div class="layer layer-1">
            <h2>
              {{ text.cardAndroidTitle }}
              <span v-if="currentPhone === 0" class="recommend-tag">{{
                text.recommend
              }}</span>
            </h2>
          </div>
          <div class="layer layer-2">
            <p>{{ text.cardAndroidDesc }}</p>
          </div>
          <div class="card-gap"></div>
          <div class="layer layer-3">
            <button class="jump-btn" type="button" @click="goAndroidStore">
              {{ text.cardAndroidBtn }}
            </button>
          </div>
        </article>

        <article class="element-two">
          <div class="layer layer-1">
            <h2>
              {{ text.cardIosTitle }}
              <span v-if="currentPhone === 1" class="recommend-tag">{{
                text.recommend
              }}</span>
            </h2>
          </div>
          <div class="layer layer-2">
            <p>{{ text.cardIosDesc }}</p>
          </div>
          <div class="card-gap"></div>
          <div class="layer layer-3">
            <button
              class="jump-btn secondary"
              type="button"
              @click="goIosStore"
            >
              {{ text.cardIosBtn }}
            </button>
          </div>
        </article>
      </div>
    </section>
  </main>
</template>

<script setup>
import { computed, onMounted, ref } from "vue";
import { n18iMessages } from "./n18i";

const androidDownloadUrl = "https://ensightful.cn/downloadApp/IoT Meter.apk";
const iphoneDownloadUrl =
  "https://apps.apple.com/cn/app/iot-meter/id6503274829";

const currentLocale = ref("zh");
const currentPhone = ref(-1);
const text = computed(() => n18iMessages[currentLocale.value]);

const toggleLocale = () => {
  currentLocale.value = currentLocale.value === "zh" ? "en" : "zh";
};

const detectLocale = () => {
  const browserLanguage = navigator.languages?.[0] || navigator.language || "";
  const normalizedLanguage = browserLanguage.toLowerCase();
  return normalizedLanguage.startsWith("zh") ? "zh" : "en";
};

const judgeCurrentPhone = () => {
  const userAgent = navigator.userAgent;

  if (userAgent.includes("Android") || userAgent.includes("Linux")) {
    return 0;
  }

  if (userAgent.includes("iPhone") || userAgent.includes("Mac")) {
    return 1;
  }

  return -1;
};

const goAndroidStore = () => {
  window.location.href = androidDownloadUrl;
};

const goIosStore = () => {
  window.location.href = iphoneDownloadUrl;
};

const downloadByDevice = () => {
  const phone = judgeCurrentPhone();
  if (phone === 0) {
    goAndroidStore();
    return;
  }

  if (phone === 1) {
    goIosStore();
    return;
  }

  // 无法判断设备时默认跳 iOS 商店页
  goIosStore();
};

onMounted(() => {
  currentPhone.value = judgeCurrentPhone();
  currentLocale.value = detectLocale();
});
</script>

<style>
html,
body,
#__nuxt {
  min-height: 100%;
  margin: 0;
  background: linear-gradient(135deg, #ffe2f1 0%, #ffe9d6 38%, #e7f4ff 100%);
}
</style>

<style scoped>
.page {
  min-height: 100vh;
  display: grid;
  place-items: center;
  padding: 20px;
  background: linear-gradient(135deg, #ffe2f1 0%, #ffe9d6 38%, #e7f4ff 100%);
  font-family: "Nunito", "PingFang SC", "Microsoft YaHei", sans-serif;
}

.element-one {
  position: relative;
  width: min(1000px, 95vw);
  min-height: 80vh;
  border-radius: 28px;
  border: 2px solid rgba(255, 255, 255, 0.75);
  background: rgba(255, 255, 255, 0.68);
  box-shadow: 0 18px 55px rgba(211, 150, 179, 0.25);
  padding: 56px 26px 26px;
  display: flex;
  flex-direction: column;
}

.lang-switch {
  position: absolute;
  top: 14px;
  right: 14px;
  border: 0;
  border-radius: 999px;
  padding: 8px 12px;
  font-size: 14px;
  font-weight: 700;
  color: #a14f85;
  background: #fff;
  cursor: pointer;
}

.page-head {
  text-align: center;
  margin-bottom: 20px;
  color: #8f6083;
  width: 100%;
}

.page-head h1 {
  margin: 0 0 8px;
  color: #9a4c81;
  font-size: clamp(24px, 3vw, 36px);
}

.page-head p {
  margin: 0;
}

.quick-download {
  margin-top: 12px;
  border: 0;
  border-radius: 20px;
  display: flex;
  align-items: center;
  justify-content: center;
  width: 100%;
  min-width: 0;
  min-height: 400px;
  padding: 18px 32px;
  background: linear-gradient(90deg, #a986ff, #c8a7ff);
  color: #fff;
  font-size: 20px;
  font-weight: 700;
  cursor: pointer;
}

.element-two-grid {
  flex: 1;
  display: grid;
  grid-template-columns: repeat(2, minmax(0, 1fr));
  gap: 18px;
}

.element-two {
  border: 2px solid rgba(255, 197, 228, 0.7);
  border-radius: 22px;
  background: rgba(255, 255, 255, 0.86);
  padding: 16px;
  display: flex;
  flex-direction: column;
  min-height: 440px;
}

.layer {
  display: flex;
  align-items: center;
  justify-content: center;
  text-align: center;
}

.layer-1 {
  flex: 0 0 20%;
}

.layer-2 {
  flex: 0 0 10%;
  color: #8f6083;
  font-size: 14px;
}

.card-gap {
  flex: 1 1 auto;
}

.layer-3 {
  flex: 0 0 60%;
}

.layer-1 h2 {
  margin: 0;
  color: #a14f85;
  font-size: clamp(22px, 2.2vw, 28px);
}

.recommend-tag {
  display: inline-block;
  margin-left: 8px;
  padding: 2px 8px;
  border-radius: 999px;
  background: rgba(255, 153, 207, 0.2);
  color: #cc4b8f;
  font-size: 12px;
  vertical-align: middle;
}

.layer-2 p {
  margin: 0;
  max-width: 260px;
}

.jump-btn {
  width: 100%;
  height: 100%;
  min-height: 48px;
  border-radius: 18px;
  display: inline-flex;
  align-items: center;
  justify-content: center;
  text-decoration: none;
  color: #fff;
  font-size: clamp(16px, 1.6vw, 22px);
  font-weight: 700;
  background: linear-gradient(90deg, #ff8fc9, #ffb9da);
  box-shadow: 0 10px 24px rgba(255, 154, 203, 0.35);
  border: 0;
  cursor: pointer;
}

.jump-btn.secondary {
  background: linear-gradient(90deg, #8ccfff, #b6e3ff);
  box-shadow: 0 10px 24px rgba(129, 195, 245, 0.35);
}

@media (max-width: 768px) {
  .page {
    padding: 12px;
  }

  .element-one {
    width: min(680px, 96vw);
    min-height: auto;
    padding: 52px 14px 14px;
    border-radius: 22px;
  }

  .page-head {
    margin-bottom: 14px;
  }

  .page-head h1 {
    font-size: clamp(20px, 7vw, 30px);
  }

  .page-head p {
    font-size: 14px;
  }

  .quick-download {
    min-width: 100%;
    min-height: 200px;
    padding: 14px 18px;
    font-size: 17px;
  }

  .element-two-grid {
    grid-template-columns: 1fr;
    gap: 14px;
  }

  .element-two {
    min-height: 380px;
    padding: 12px;
  }

  .layer-1 h2 {
    font-size: 22px;
  }

  .layer-2 {
    font-size: 13px;
  }

  .lang-switch {
    top: 10px;
    right: 10px;
    font-size: 13px;
  }
}
</style>
