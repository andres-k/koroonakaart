<template>
  <b-container>
    <highcharts class="chart" :options="chartOptions"></highcharts>
  </b-container>
</template>

<script>
import data from "../../data.json";

export default {
  name: "PositiveNegativeChart",

  data() {
    return {
      chartType: "percent",

      chartOptions: {
        title: {
          text: this.$t("positiveNegativeTitle"),
          align: "left",
          y: 30
        },

        chart: {
          type: "column",
          height: 470,
          events: {
            load: function() {
              // Buttons have indexes go in even numbers (button1 [0], button2 [2])
              // Odd indexes are button symbols
              const button = this.exportSVGElements[4];

              // States:
              // 0 - normal
              // 1 - hover
              // 2 - selected
              // 3 - disabled
              button.setState(2);
            }
          }
        },

        exporting: {
          buttons: {
            customButton: {
              text: this.$t("abs"),
              onclick: function() {
                this.chartType = "abs";
                const button1 = this.exportSVGElements[2];
                const button2 = this.exportSVGElements[4];

                button1.setState(this.chartType === "abs" ? 2 : 0);
                button2.setState(this.chartType === "percent" ? 2 : 0);

                this.update({
                  plotOptions: {
                    column: {
                      stacking: "normal"
                    }
                  },
                  yAxis: {
                    title: {
                      text: "Abs"
                    }
                  }
                });
              }
            },
            customButton2: {
              text: "%",
              onclick: function() {
                this.chartType = "percent";
                const button1 = this.exportSVGElements[2];
                const button2 = this.exportSVGElements[4];

                button1.setState(this.chartType === "abs" ? 2 : 0);
                button2.setState(this.chartType === "percent" ? 2 : 0);

                this.update({
                  plotOptions: {
                    column: {
                      stacking: "percent"
                    }
                  },
                  yAxis: {
                    title: {
                      text: "%"
                    }
                  }
                });
              }
            }
          }
        },

        // Remove Highcharts.com link from bottom right
        credits: {
          enabled: false
        },
        navigation: {
          buttonOptions: {
            verticalAlign: "top",
            y: -15,
            theme: {
              fill: "none",
              stroke: "none",
              "stroke-width": 0,
              r: 4,

              states: {
                hover: {
                  /* fill: "#f5f5f5" */
                },
                select: {
                  fill: "none",
                  style: {
                    fontWeight: "bold",
                    textDecoration: "underline"
                  }
                }
              },
              style: {
                /* color: "#039", */
                /* fontWeight: "bold", */
                textDecoration: "none"
              }
            }
          }
        },
        xAxis: {
          labels: {
            style: {
              fontSize: "13px",
              fontWeight: "bold"
            }
          },
          categories: [
            this.$t("insufficientData"),
            "Harjumaa",
            "Hiiumaa",
            "Ida-Virumaa",
            "Jõgevamaa",
            "Järvamaa",
            "Läänemaa",
            "Lääne-Virumaa",
            "Põlvamaa",
            "Pärnumaa",
            "Raplamaa",
            "Saaremaa",
            "Tartumaa",
            "Valgamaa",
            "Viljandimaa",
            "Võrumaa"
          ]
        },

        yAxis: {
          title: {
            text: "%"
          }
        },
        tooltip: {
          headerFormat:
            '<span style="font-size:10px">{point.key}</span><table>',
          pointFormat:
            '<tr><td style="color:{series.color};padding:0">{series.name}: </td>' +
            '<td style="padding:0"><b>{point.y}</b> ({point.percentage:.0f}%)</td></tr>',
          footerFormat: "</table>",
          shared: true,
          useHTML: true
        },
        plotOptions: {
          column: {
            stacking: "percent",
            enableMouseTracking: true
          }
        },

        series: [
          {
            name: this.$t("negative"),
            data: data.dataPositiveNegativeChart.negative,
            color: "#A6C96A"
          },
          {
            name: this.$t("positive"),
            data: data.dataPositiveNegativeChart.positive,
            color: "#910000"
          }
        ]
      }
    };
  },

  // Get current locale
  computed: {
    currentLocale: function() {
      return this.$i18n.locale;
    }
  },

  // Fire when currentLocale computed property changes
  watch: {
    currentLocale() {
      this.chartOptions.title.text = this.$t("positiveNegativeTitle");
      //  this.chartOptions.yAxis.title.text = this.$t("numberOfCases");
      this.chartOptions.series[0].name = this.$t("negative");
      this.chartOptions.series[1].name = this.$t("positive");
      this.chartOptions.xAxis.categories[0] = this.$t("insufficientData");
      this.chartOptions.exporting.buttons.customButton.text = this.$t("abs");
    }
  }
};
</script>

<style lang="scss" scoped>
</style>
