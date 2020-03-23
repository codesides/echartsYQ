<template>
  <div ref="main" id="main" style="width: 1000px;height:600px;"></div>
</template>

<script>
import echarts from "echarts";
import "echarts/map/js/china.js";
import jsonp from "jsonp";

const options = {
  title: {
    text: "疫情地图"
  },
  series: [
    {
      type: "map",
      map: "china",
      name: "确诊人数",
      label: {
        //控制地区汉子
        show: true,
        color: "#333",
        fontSize: 12
      },
      itemStyle: {
        //地图板块的颜色和边框
        areaColor: "#eee",
        borderColor: "#666"
      },
      zoom: 1.2,
      emphasis: {
        //鼠标划过区域样式
        label: {
          color: "white",
          fontSize: 14
        },
        itemStyle: {
          areaColor: "#f50",
          borderColor: "#666"
        }
      },
      data: []
    }
  ],
  tooltip: {
    trigger: "item"
  },
  toolbox: {
    show: true,
    orient: "vertical",
    left: "right",
    top: "center",
    feature: {
      dataView: { readOnly: false },
      restore: {},
      saveAsImage: {}
    }
  },
  visualMap: [
    {
      type: "piecewise",
      pieces: [
        { min: 10000 }, // 不指定 max，表示 max 为无限大（Infinity）。
        { min: 1000, max: 9999 },
        { min: 100, max: 999 },
        { min: 10, max: 99 },
        { max: 10 } // 不指定 min，表示 min 为无限大（-Infinity）。
      ],
      color: ["red", "#f6efa6"]
    }
  ]
};

export default {
  name: "HelloWorld",
  data() {
    return {};
  },
  mounted() {
    this.setEcharts();
    // this.getData();
  },
  created() {
    this.getData();
  },
  methods: {
    setEcharts() {
      this.myEcharts = echarts.init(this.$refs.main);

      this.myEcharts.setOption(options);
    },
    getData() {
      jsonp(
        "https://interface.sina.cn/news/wap/fymap2020_data.d.json?_=2580892522427",
        {},
        (err, data) => {
          if (!err) {
            console.log(data);
            let list = data.data.list.map(item => ({
              name: item.name,
              value: item.value
            }));
            options.series[0].data = list;
            this.myEcharts.setOption(options);
          }
        }
      );
    }
  }
};
</script>
<style scoped></style>
