<script>
import { Line } from 'vue-chartjs';
import moment from 'moment';
export default {
  extends: Line,
  methods: {
      initChart:function(countryReport){
          const vim = this;
                this.renderChart({
                    labels: countryReport.map(({ Date })=>  moment(String(Date)).format('MM/DD/YYYY')),
                    datasets:[{
                        data:countryReport.map( ({ Confirmed })=> Confirmed),
                        label: 'Confirmed',
                        borderColor: 'blue',
                        fill:true,
                    },{
                        data:countryReport.map( ({ Deaths })=> Deaths),
                        label: 'deaths',
                        borderColor: 'red',
                        backgroundColor: 'rgba(255,0,0,0.5)',
                        fill:true,
                    },{
                        data:countryReport.map( ({ Recovered })=> Recovered),
                        label: 'Recovered',
                        borderColor: 'green',
                        backgroundColor: 'rgb(0 255 165 / 50%)',
                        fill:true,
                    }]
                });
      }
  },    
}
</script>

<style>
</style>