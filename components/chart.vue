<template>
  <div></div>
</template>

<script>
export default {
  props: ['dataBTC'],
  mounted() {
    console.log(this.dataBTC)
    var chart = LightweightCharts.createChart(document.body, {
      width: 1200,
      height: 600,
      layout: {
        textColor: '#d1d4dc',
        backgroundColor: '#000000',
      },
      rightPriceScale: {
        scaleMargins: {
          top: 0.3,
          bottom: 0.25,
        },
      },
      crosshair: {
        vertLine: {
          width: 5,
          color: 'rgba(224, 227, 235, 0.1)',
          style: 0,
        },
        horzLine: {
          visible: false,
          labelVisible: false,
        },
      },
      grid: {
        vertLines: {
          color: 'rgba(42, 46, 57, 0)',
        },
        horzLines: {
          color: 'rgba(42, 46, 57, 0)',
        },
      },
    })

    var areaSeries = chart.addAreaSeries({
      topColor: 'rgba(38, 198, 218, 0.56)',
      bottomColor: 'rgba(38, 198, 218, 0.04)',
      lineColor: 'rgba(38, 198, 218, 1)',
      lineWidth: 2,
      crossHairMarkerVisible: false,
    })

    areaSeries.setData(this.dataBTC)

    document.body.style.position = 'relative'

    var legend = document.createElement('div')
    legend.classList.add('legend')
    document.body.appendChild(legend)

    var firstRow = document.createElement('div')
    firstRow.innerText = 'BTC USD'
    firstRow.style.color = 'white'
    legend.appendChild(firstRow)

    function pad(n) {
      var s = '0' + n
      return s.substr(s.length - 2)
    }

    chart.subscribeCrosshairMove((param) => {
      if (param.time) {
        const price = param.seriesPrices.get(areaSeries)
        firstRow.innerText = 'BTC USD' + '  ' + price.toFixed(2)
      } else {
        firstRow.innerText = 'BTC USD'
      }
    })
  },
}
</script>
