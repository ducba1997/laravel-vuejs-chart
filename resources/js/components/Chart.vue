<template>
  <div class="TVChartContainer" :id="containerId" />
</template>

<script>
import { widget } from "../../../public/js/charting_library";

export default {
  name: "Chart",
  props: {
    symbol: {
      default: "VNM",
      type: String,
    },
    interval: {
      default: "15",
      type: String,
    },
    containerId: {
      default: "tv_chart_container",
      type: String,
    },
    datafeedUrl: {
      default: "http://127.0.0.1:8000/api",
      type: String,
    },
    libraryPath: {
      default: "/js/charting_library/",
      type: String,
    },
    chartsStorageUrl: {
      default: "https://saveload.tradingview.com",
      type: String,
    },
    chartsStorageApiVersion: {
      default: "1.1",
      type: String,
    },
    clientId: {
      default: "tradingview.com",
      type: String,
    },
    userId: {
      default: "public_user_id",
      type: String,
    },
    fullscreen: {
      default: true,
      type: Boolean,
    },
    autosize: {
      default: true,
      type: Boolean,
    },
    studiesOverrides: {
      type: Object,
    },

  },
  tvWidget: null,
  mounted() {
    const widgetOptions = {
      symbol: this.symbol,
      // BEWARE: no trailing slash is expected in feed URL
      datafeed: new window.Datafeeds.UDFCompatibleDatafeed(this.datafeedUrl),
      interval: this.interval,
      container_id: this.containerId,
      library_path: this.libraryPath,

      locale: "vi",
      disabled_features: ["use_localstorage_for_settings"],
      enabled_features: ["study_templates"],
      charts_storage_url: this.chartsStorageUrl,
      charts_storage_api_version: this.chartsStorageApiVersion,
      client_id: this.clientId,
      user_id: this.userId,
      time_frames: [
        { text: "1y", resolution: "12M", description: "Năm",title: "Năm" },
        { text: "1m", resolution: "1M", description: "Tháng", title: "Tháng" },
        { text: "1w", resolution: "1W", description: "Tuần", title: "Tuần" },
        { text: "1d", resolution: "1D", description: "Ngày", title: "Ngày" },
        { text: "1h", resolution: "60", description: "Giờ", title: "Giờ" },
      ],
      fullscreen: this.fullscreen,
      autosize: this.autosize,
      studies_overrides: this.studiesOverrides,
    };

    const tvWidget = new widget(widgetOptions);
    this.tvWidget = tvWidget;
  },
  destroyed() {
    if (this.tvWidget !== null) {
      this.tvWidget.remove();
      this.tvWidget = null;
    }
  },
};
</script>

<style lang="scss" scoped>
.TVChartContainer {
  height: calc(100vh - 80px);
}
</style>
