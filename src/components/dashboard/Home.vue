<template>
  <div id="dashboard">
    <div class="row clearfix">
      <div class="col-lg-4 col-md-6 col-sm-12">
        <div class="card widget_2 big_icon traffic">
          <div class="body">
            <h6>Out of Stock</h6>
            <h2>20 <small class="info">Item</small></h2>
            <small>2% higher than last month</small>
            <div class="progress">
              <div class="progress-bar l-amber" role="progressbar" aria-valuenow="45" aria-valuemin="0"
                aria-valuemax="100" style="width: 45%;"></div>
            </div>
          </div>
        </div>
      </div>
      <div class="col-lg-4 col-md-6 col-sm-12">
        <div class="card widget_2 big_icon sales">
          <div class="body">
            <h6>Sales</h6>
            <h2>12 <small class="info">Item sale</small></h2>
            <small>6% higher than last month</small>
            <div class="progress">
              <div class="progress-bar l-blue" role="progressbar" aria-valuenow="38" aria-valuemin="0" aria-valuemax="100"
                style="width: 38%;"></div>
            </div>
          </div>
        </div>
      </div>
      <div class="col-lg-4 col-md-6 col-sm-12">
        <div class="card widget_2 big_icon balance2">
          <div class="body">
            <h6>Revenue</h6>
            <h2>Rp21.000.000 <small class="info"></small></h2>
            <small>6% higher than last month</small>
            <div class="progress">
              <div class="progress-bar l-blue" role="progressbar" aria-valuenow="38" aria-valuemin="0" aria-valuemax="100"
                style="width: 38%;"></div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
// @ is an alias to /src
export default {
  name: 'Home',
  components: {},
  created () {
    this.$store.dispatch('changePageTitle', 'Dashboard')
  }
}
</script>
