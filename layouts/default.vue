<template>
  <div id="defaultLayout">
    <div
      id="initialCover"
      style="width:100vw;height:100vh;background-color:#fff;position:fixed;top:0;left:0;z-index:2000;"
    ></div>
    <NoIe class="secondBody">
      <div class="ie" v-if="ie">{{ $t("ie") }}</div>
    </NoIe>
    <NavL @click="goHome" class="secondBody" />
    <NavR v-show="$vssWidth > $data.$tab" class="secondBody" />
    <MainMenu v-show="$vssWidth <= $data.$tab" />
    <TheHeader id="bagus-top" />
    <main class="firstBody">
      <BagusTitle v-show="$vssWidth > $data.$tab" />
      <client-only>
        <BagusWalls v-if="$vssWidth > $data.$tab" />
      </client-only>
      <nuxt class="bagus-box" />
    </main>
    <client-only>
      <Contact id="contact" v-if="$vssWidth > $data.$tab" class="secondBody" />
    </client-only>
    <TheFooter class="firstBody" />
  </div>
</template>

<style lang="scss">
// #initialCover {
// width=100vw;height=100vh;background-color:#eee;position:fixed;top:0;left:0;z-index:2000;
// }
#defaultLayout {
  background-color: $green-p;
  max-width: 100vw;
  border: 0.7rem solid $gray-d;
  display: flex;
  flex-direction: column;
  align-items: center;
  overflow: hidden;

  @include respond("tab") {
    border: none;
    padding: 3rem 2.5rem 0;
    min-height: 100vh;
    justify-content: flex-end;
  }
  @include respond("iphoneX") {
    padding: 3rem 0 0;
  }
}
main {
  height: 97vh;
  width: 100%;
  max-width: 150vh;
  max-height: 66.67vw;
  background-color: $white-p;
  border: 1.2rem solid $gray-d;
  border-top: none;
  position: relative;

  @include respond("wide-scr") {
    max-width: none;
    width: 75%;
    height: auto;
    margin-top: -0.5rem;
  }
  @include respond("tab") {
    height: auto;
    max-height: none;
    margin: -0.5rem 0 3rem;
    border: 0.8rem solid $gray-d;
    border-top: none;
  }
  @include respond("mobile") {
    margin-top: -0.7rem;
  }
}

.bagus-box {
  height: 100%;
  width: 101%;
  margin-top: 0.6rem;
  margin-left: -0.5%;

  @include respond("tab") {
    margin-top: 0;
    height: auto;
  }
}

.ie {
  position: absolute;
  top: 8rem;
  right: 1rem;
}

.noscript {
  position: absolute;
  top: 10rem;
  left: 1rem;
  z-index: 1000;
  white-space: pre;
  border: 1px solid #333;
  box-shadow: 2px 2px 4px rgba(0, 0, 0, 0.2);
  padding: 0.9rem 1.3rem;
  background-color: rgba(255, 255, 255, 0.7);
  border-radius: 3px;
  font-size: 1.3rem;
  text-align: center;
}
</style>

<script>
import NavL from "~/components/Top/NavL";
import NavR from "~/components/Top/NavR";
import TheHeader from "~/components/Header/TheHeader";
import BagusWalls from "~/components/Top/BagusWalls";
import BagusTitle from "~/components/Top/BagusTitle";
import Contact from "~/components/Contact/Contact";
import TheFooter from "~/components/GlobalComponents/TheFooter";

import detectIE from "~/mixins/detectIE.js";
import { TweenMax, TimelineLite } from "gsap";
import { setTimeout } from "timers";

export default {
  head() {
    const i18nSeo = this.$nuxtI18nSeo();
    const pageDesc = this.$t("desc.portfolio");
    const url = "https://oliancho.com";
    return {
      titleTemplate: "%s | OliAncho",
      htmlAttrs: { ...i18nSeo.htmlAttrs },
      meta: [
        { hid: "charset", charset: "utf-8" },
        {
          name: "viewport",
          hid: "viewport",
          content: "width=device-width, initial-scale=1"
        },
        {
          hid: "format-detection",
          name: "format-detection",
          content: "telephone=no, email=no, address=no"
        },
        {
          hid: "description",
          name: "description",
          content: pageDesc
        },
        { property: "og:title", content: "OliAncho" },
        { property: "og:type", content: "website" },
        { property: "og:site_name", content: "OliAncho" },
        { property: "og:url", content: url },
        {
          property: "og:description",
          content: pageDesc
        },
        {
          property: "og:image",
          content: url + "/images/top-thumbnail.jpg"
        },
        {
          property: "og:image:width",
          content: "1200"
        },
        {
          property: "og:image:height",
          content: "1200"
        },
        { name: "twitter:card", content: "summary_large_image" },
        ...i18nSeo.meta
      ],
      link: [...i18nSeo.link]
    };
  },
  components: {
    NavL,
    NavR,
    TheHeader,
    BagusWalls,
    BagusTitle,
    Contact,
    TheFooter,
    MainMenu: () => import("~/components/Mobile/MainMenu"),
    NoIe: () => import("~/components/GlobalComponents/NoIe")
  },
  mixins: [detectIE],
  methods: {
    goHome() {
      if (!this.$route.name.startsWith("index"))
        this.$router.push(this.localePath("index"));
    },
    intro() {
      document.body.style.overflow = "hidden";
      const tl = new TimelineLite({
        onComplete: () => {
          document.body.style.overflow = "visible";
        }
      });
      tl.to("#initialCover", 0.3, { display: "none" })
        .from(".logo-box", 0.8, { y: -1000, ease: Bounce.easeOut }, "-=0.2")
        .from(".firstBody", 1.4, { autoAlpha: 0, ease: Power1.easeIn })
        .from(
          "#defaultLayout",
          1.4,
          {
            backgroundColor: "#fff",
            borderColor: "#fff",
            ease: Power1.easeIn
          },
          "-=1"
        )
        .from(".secondBody", 0.2, { autoAlpha: 0, ease: Power1.easeOut });
    }
  },
  mounted() {
    if (process.client) this.$scrollTo("#defaultLayout");
    this.intro();
  },
  destroyed() {
    document.getElementById("initialCover").style.display = "block";
  }
};
</script>
