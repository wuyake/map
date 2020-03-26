<template>
  <div>
    <div class="echarts" ref="echarts"></div>
  </div>
</template>

<script>
import echarts from 'echarts'
import 'echarts/map/js/china.js'
import jsonp from 'jsonp'
const option = {
  series:[{
	  name:'感染人数',
      type:'map',
      map:'china',
      zoom:1.2,
      label:{
        show:true,
        // color:'#f40',
        fontSize:12
      },
      itenStyle:{

      },
      emphasis:{},
      data:[{}]
  }],
  roam:true,
  visualMap: [{
				type: 'piecewise',
				show: true,
				splitNumber: 6,
				pieces: [{min: 10000}, // 不指定 max，表示 max 为无限大（Infinity）。
						{
							min: 1000,
							max: 9999
						},
						{
							min: 100,
							max: 999
						},
						{
							min: 10,
							max: 99
						},
						{
							min: 1,
							max: 9
						},
						{
							min: 0,
							max: 0
						}
					],
					//align:'right' // 默认是left
					inRange: {
						symbol: 'rect',
						color: ['#eee', '#ffaa85', '#ff7b69', '#cc2929', '#8c0d0d', '#660208']
					},
					itemHeight: 10,
					itemWidth: 20
			 }],
	tooltip:{
		trigger:'item'
	}
		
}
export default {
  name: 'Map',
  data(){
	  return{
		  list:[]
	  }
  },
  mounted(){
	  this.getData()
    this.mychart = echarts.init(this.$refs.echarts);
    this.mychart.setOption(option)
  },
  methods:{
	  getData(){
		  jsonp('http://interface.sina.cn/news/wap/fymap2020_data.d.json?_=1580892522427',{},(err,data)=>{
			  if(! err){
				  this.list = data.data.list.map((item)=>{
					  return{
						  name:item.name,
						  value:item.value
					  }
				  },0)
				  option.series[0].data = this.list
				  console.log(this.list)
				  this.mychart.setOption(option)
			  }
		  })
	  }
  }
 
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.echarts {
  width: 900px;
  height: 600px;
}
</style>
