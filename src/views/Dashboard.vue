<template>
  <div id='Landing'>
    <Sidebar/>
    <section class="content">
      <div class="body_scroll">
        <div class="block-header">
          <div class="row">
            <div class="col-lg-7 col-md-6 col-sm-12">
              <h2>{{ pageTitle }}</h2>
              <ul class="breadcrumb">
                <li class="breadcrumb-item">
                  <a href="#"><i class="zmdi zmdi-home"></i> Home</a>
                </li>

                <li class="breadcrumb-item active">Product List</li>
              </ul>
              <button
                class="btn btn-primary btn-icon mobile_menu"
                type="button"
              >
                <i class="zmdi zmdi-sort-amount-desc"></i>
              </button>
            </div>
            <div class="col-lg-5 col-md-6 col-sm-12">
              <button
                class="btn btn-primary btn-icon float-right right_icon_toggle_btn"
                type="button"
              >
                <i class="zmdi zmdi-arrow-right"></i>
              </button>
              <button
                @click.prevent="logout"
                class="btn btn-danger btn-icon float-right"
                type="button"
                title="Logout"
              >
                <i class="zmdi zmdi-power"></i>
              </button>
              <button
                v-if="buttonAdd === 'product'"
                @click.prevent="displayModal(true, 'addProduct')"
                class="btn btn-success btn-icon float-right"
                type="button"
                title="Add New Product"
              >
                <i class="zmdi zmdi-plus"></i>
              </button>
              <button
                v-if="buttonAdd === 'banner'"
                @click.prevent="displayModal(true, 'addBanner')"
                class="btn btn-success btn-icon float-right"
                type="button"
                title="Add New Banner"
              >
                <i class="zmdi zmdi-plus"></i>
              </button>
            </div>
          </div>
        </div>
        <!-- Content Menu -->
        <router-view />
      </div>
    </section>

    <AddProduct
    v-if="modalName === 'addProduct'"
    v-show="showModal"
    @isDisplayModal="isDisplayModal" />

    <AddBanner
    v-else-if="modalName === 'addBanner'"
    v-show="showModal"
    @isDisplayModal="isDisplayModal" />
  </div>
</template>

<script>
// @ is an alias to /src
import Sidebar from '@/components/dashboard/Sidebar.vue'
import AddProduct from '@/components/product/AddProduct.vue'
import AddBanner from '@/components/banner/AddBanner.vue'

export default {
  name: 'Dashboard',
  components: {
    Sidebar,
    AddProduct,
    AddBanner
  },
  computed: {
    title () {
      return this.$store.state.title
    },

    pageTitle () {
      return this.$store.state.pageTitle
    }
  },

  data () {
    return {
      buttonAdd: '',
      showModal: false,
      modalName: '',
      loading: false
    }
  },

  created () {
    this.showButton()
    this.$store.dispatch('getCategories')
  },

  updated () {
    this.showButton()
  },

  methods: {
    isDisplayModal (params) {
      this.showModal = params
      this.modalName = ''
    },

    displayModal (show, modalName) {
      this.showModal = true
      this.modalName = modalName
    },

    showButton () {
      if (this.$route.name === 'Product') {
        this.buttonAdd = 'product'
      } else if (this.$route.name === 'Banner') {
        this.buttonAdd = 'banner'
      } else {
        this.buttonAdd = ''
      }
    },

    logout () {
      console.log('Logout')
      localStorage.clear()
      this.$router.push({ name: 'Login' })
    }
  }
}
</script>

<style>
</style>
