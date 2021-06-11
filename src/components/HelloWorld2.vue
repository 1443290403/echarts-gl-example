<template>
  <div>
      <div id="mapContainer" style="width:800px;height:800px;margin:0 auto;"></div>
  </div>
</template>

<script>
export default {
    mounted() {
        this.$nextTick(()=>{
            this.getData()
        })
    },
    methods: {

        makeGaussian(amplitude,x0,y0,sigmaX,sigmaY){
            return function(amplitude,x0,y0,sigmaX,sigmaY,x,y){
                var exponent = -(
                    Math.pow(x - x0, 2) / (2 * Math.pow(sigmaX, 2)) +
                    Math.pow(y - y0, 2) / (2 * Math.pow(sigmaY, 2)) 
                );
                return amplitude * Math.pow(Math.E, exponent);
            }.bind(null,amplitude,x0,y0,sigmaX,sigmaY);
        },
        getData() {
            var myChart;
            
            var gaussian = this.makeGaussian(50,0,0,20,20);
            var data = [];
            for(var i = 0; i < 1000; i++){
                var x = Math.random() * 100 - 50;
                var y = Math.random() * 100 - 50;
                var z = gaussian(x,y);
                data.push([x,y,z]);
            }
            var option = {
                grid3D:{
                    left:'center',
                    top:'center'
                },
                xAxis3D:{},
                yAxis3D:{},
                zAxis3D:{ max:100},
                series:[
                    {
                        type:"scatter3D",
                        data:data
                    }
                ]
            }
            this.$nextTick(()=>{
                myChart = this.$echarts.init(document.getElementById('mapContainer'));
                myChart.setOption(option);
            })
            
        }
    },
}
</script>

<style>

</style>