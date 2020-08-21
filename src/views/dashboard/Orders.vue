<template>
  <div>
    <h2>訂單列表</h2>

    <div class="container mt-3">
      <div>
        <Loading :active.sync="isLoading"></Loading>
        <table class="table mt-4">
          <thead>
            <tr>
              <th width="150">日期</th>
              <th width="300">購買項目</th>
              <th width="120">購買方式</th>
              <th class="text-right" width="120">金額</th>
              <th width="60">已付款</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="(item) in orders" :key="item.id">
              <td>{{ item.created.datetime }}</td>
              <td>
                <li v-for="(product) in item.products" :key="product.title">
                  {{ product.product.title }}
                  ({{ product.quantity }} {{ product.product.unit }})
                </li>
              </td>
              <td>{{ item.payment }}</td>
              <td class="text-right">{{ item.amount }}</td>
              <td>
                <span v-if="item.paid" class="text-success">是</span>
                <span v-else>否</span>
              </td>
            </tr>
          </tbody>
        </table>
        <pagination :pages="pages" v-on:emit-pages="getorders"></pagination>
      </div>
    </div>
  </div>
</template>
<script>
import Pagination from '../Pagination.vue';
import $ from 'jquery';

export default {
  components: {
    Pagination,
  },
  created() {
    this.getorders(1);
  },
  props: ['token'],
  methods: {
    refreshScreen() {
      this.getorders(this.pages.current_page);
    },
    getorders(page = 1) {
      this.isLoading = true;

      //let api = `https://course-ec-api.hexschool.io/api/${this.user.uuid}/ec/orders?page=${page}`;
      const api = `${process.env.VUE_APP_APIPATH}api/${process.env.VUE_APP_UUID}/ec/orders?page=${page}`;

      this.$http
        .get(api)
        .then((response) => {
          this.isLoading = false;
          //console.log(response);
          console.log(response.data.data);
          //this.orders = JSON.parse(JSON.stringify(response.data.data));
          this.orders = response.data.data;

          this.pages.current_page = response.data.meta.pagination.current_page;
          this.pages.total_pages = response.data.meta.pagination.total_pages;
          //this.pages.total_pages = 5;

          //this.pages.current_page = 2;
          //this.pages.total_pages = 5;

          console.log(this.orders);
        })
        .catch((error) => {
          console.log(error);
          this.isLoading = false;
        });
    },
  },
  data() {
    return {
      isLoading: false,
      title: '',
      orders: [],
      tempProduct: {
        imageUrl: [],
      },
      tempIndex: 0,
      user: {
        token: '',
        uuid: '',
      },
      pages: { current_page: 1, total_pages: 0 },
    };
  },
};
</script>
