<template>
  <div id="list-banner">
    <Loading :loading="loading"/>
    <div class="container-fluid">
      <div class="row clearfix">
        <div class="col-lg-12">
          <div class="card">
            <div class="table-responsive">
              <table
                class="table table-hover product_item_list c_table theme-color mb-0"
              >
                <thead>
                  <tr>
                    <th>Image</th>
                    <th>Name</th>
                    <th data-breakpoints="xs md">Star Date</th>
                    <th data-breakpoints="xs md">End Date</th>
                    <th data-breakpoints="xs">Status</th>
                    <th data-breakpoints="xs md">Category</th>
                    <th data-breakpoints="sm xs md">Action</th>
                  </tr>
                </thead>
                <tbody>
                  <tr v-for="banner in banners" :key="banner.id">
                    <td>
                      <img
                        src="https://www.kindpng.com/picc/m/465-4653741_e-commerce-banner-ecommerce-web-development-services-hd.png"
                        width="48"
                        alt="Banner Image"
                        class="img-fluid img-thumbnail"
                      />
                    </td>
                    <td><h5>{{ banner.name }}</h5></td>
                    <td>{{ formatDate(banner.start_date) }}</td>
                    <td>{{ formatDate(banner.end_date) }}</td>
                    <td><strong><span :class="getColor(banner.is_active)" style="font-size: 0.9rem; padding: 6px">{{ getStatus(banner.is_active) }}</span></strong></td>
                    <td><strong><span :class="getColorCategory(banner.Category)"> {{ getCategory(banner.Category) }} </span></strong></td>
                    <td>
                      <button
                      @click.prevent="editBanner(banner.id)"
                        class="btn btn-primary waves-effect waves-float btn-sm waves-green"
                        ><i class="zmdi zmdi-edit"></i
                      ></button>
                      <button
                        @click.prevent="confirmDelete(banner.id)"
                        class="btn btn-danger waves-effect waves-float btn-sm waves-red"
                        ><i class="zmdi zmdi-delete"></i
                      ></button>
                    </td>
                  </tr>
                </tbody>
              </table>
            </div>
          </div>
        </div>
      </div>
    </div>
    <EditBanner
    v-if="showModal"
    v-show="showModal"
    @isDisplayModal="isDisplayModal"
    />
  </div>
</template>

<script>
import Loading from '@/components/loading/Loading.vue'
import axios from '@/config/axios'
import Swal from 'sweetalert2'
import EditBanner from '@/components/banner/EditBanner.vue'

export default {
  name: 'Banner',
  data () {
    return {
      loading: false,
      showModal: false,
      banner: {}
    }
  },
  components: {
    Loading, EditBanner
  },
  computed: {
    banners () {
      return this.$store.state.banners
    }
  },
  created () {
    this.loading = true
    this.$store.dispatch('changePageTitle', 'Banner List')
    this.getBanners()
  },
  methods: {

    async getBanners () {
      await this.$store.dispatch('getBanners')
      this.loading = false
    },
    async editBanner (bannerId) {
      try {
        this.loading = true
        const result = await this.$store.dispatch('getBannerById', bannerId)
        this.banner = result
        this.showModal = true
      } catch (error) {
        console.log(error)
      } finally {
        this.loading = false
      }
    },
    confirmDelete (bannerId) {
      Swal.fire({
        title: 'Are you sure?',
        text: "You won't be able to revert this!",
        icon: 'warning',
        showCancelButton: true,
        confirmButtonColor: '#3085d6',
        cancelButtonColor: '#d33',
        confirmButtonText: 'Yes, delete it!'
      }).then((result) => {
        if (result.isConfirmed) {
          this.loading = true
          axios({
            url: 'banners/' + bannerId,
            method: 'delete',
            headers: {
              access_token: localStorage.getItem('access_token')
            }
          })
            .then((result) => {
              this.$store.dispatch('getBanners')
              Swal.fire('Success', 'Success deleted banner!', 'success')
            }).catch((error) => {
              const message = error.response.data.message || 'Somthing error'
              Swal.fire('Delete Failed', message, 'error')
              console.log(error.response)
            }).then(() => {
              this.loading = false
            })
        }
      })
    },
    isDisplayModal (params) {
      this.showModal = false
    },
    getStatus (status) {
      return (status === 'true') ? 'Active' : 'Inactive'
    },
    getColor (status) {
      return (status === 'true') ? 'badge badge-success' : 'badge badge-default'
    },
    getColorCategory (category) {
      return (category === null) ? 'col-red' : 'text-dark'
    },
    getCategory (category) {
      return (category !== null) ? category.name : 'No Category'
    },
    formatDate (date) {
      const month = ['Jan', 'Feb', 'March', 'Apr', 'May', 'Jun', 'Jul', 'Aug', 'Sep', 'Oct', 'Nov', 'Dec']
      const newDate = new Date(date)
      return newDate.getDate() + ' ' + month[newDate.getMonth()] + ' ' + newDate.getFullYear()
    }
  }
}
</script>

<style>

</style>
