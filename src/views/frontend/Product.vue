<template>
  <div>
    <h2>單一產品頁面</h2>

    <div class="container mt-3">
      <div>
        <Loading :active.sync="isLoading"></Loading>
        <div class="modal-dialog modal-xl" role="document">
          <div class="modal-content border-0">
            <div class="modal-header bg-dark text-white">
              <h5 id="exampleModalLabel" class="modal-title">
                <span>產品細節</span>
              </h5>
              <button type="button" class="close" data-dismiss="modal" aria-label="Close">
                <span aria-hidden="true">&times;</span>
              </button>
            </div>
            <div class="modal-body">
              <div class="row">
                <div class="col-sm-4">
                  <div class="form-group">
                    <label for="imageUrl">輸入圖片網址</label>
                    <input
                      readonly
                      id="imageUrl"
                      v-model="tempProduct.imageUrl[0]"
                      type="text"
                      class="form-control"
                      placeholder="請輸入圖片連結"
                    />
                  </div>
                  <img class="img-fluid" :src="tempProduct.imageUrl" alt />
                </div>
                <div class="col-sm-8">
                  <div class="form-group">
                    <label for="title">標題</label>
                    <input
                      readonly
                      id="title"
                      v-model="tempProduct.title"
                      type="text"
                      class="form-control"
                      placeholder="請輸入標題"
                    />
                  </div>

                  <div class="form-row">
                    <div class="form-group col-md-6">
                      <label for="category">分類</label>
                      <input
                        readonly
                        id="category"
                        v-model="tempProduct.category"
                        type="text"
                        class="form-control"
                        placeholder="請輸入分類"
                      />
                    </div>
                    <div class="form-group col-md-6">
                      <label for="price">單位</label>
                      <input
                        readonly
                        id="unit"
                        v-model="tempProduct.unit"
                        type="unit"
                        class="form-control"
                        placeholder="請輸入單位"
                      />
                    </div>
                  </div>

                  <div class="form-row">
                    <div class="form-group col-md-6">
                      <label for="origin_price">原價</label>
                      <input
                        readonly
                        id="origin_price"
                        v-model="tempProduct.origin_price"
                        type="number"
                        class="form-control"
                        placeholder="請輸入原價"
                      />
                    </div>
                    <div class="form-group col-md-6">
                      <label for="price">售價</label>
                      <input
                        readonly
                        id="price"
                        v-model="tempProduct.price"
                        type="number"
                        class="form-control"
                        placeholder="請輸入售價"
                      />
                    </div>
                  </div>
                  <hr />

                  <div class="form-group">
                    <label for="description">產品描述</label>
                    <textarea
                      readonly
                      id="description"
                      v-model="tempProduct.description"
                      type="text"
                      class="form-control"
                      placeholder="請輸入產品描述"
                    ></textarea>
                  </div>
                  <div class="form-group">
                    <label for="content">說明內容</label>
                    <textarea
                      readonly
                      id="description"
                      v-model="tempProduct.content"
                      type="text"
                      class="form-control"
                      placeholder="請輸入說明內容"
                    ></textarea>
                  </div>
                  <div class="form-group">
                    <div class="form-check">
                      <input
                        disabled
                        id="is_enabled"
                        v-model="tempProduct.is_enabled"
                        class="form-check-input"
                        type="checkbox"
                        :true-value="1"
                        :false-value="0"
                      />
                      <label class="form-check-label" for="is_enabled">是否啟用</label>
                    </div>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>
<script>
import $ from 'jquery';

export default {
  created() {
    this.getProduct();
  },
  props: ['token'],
  methods: {
    getProduct() {
      this.isLoading = true;
      //let api = `https://course-ec-api.hexschool.io/api/${this.user.uuid}/ec/products?page=${page}`;
      const api = `${process.env.VUE_APP_APIPATH}api/${process.env.VUE_APP_UUID}/ec/product/${this.$route.params.id}`;

      this.$http
        .get(api)
        .then((response) => {
          this.isLoading = false;
          //console.log(response);
          console.log(response.data.data);
          //this.products = JSON.parse(JSON.stringify(response.data.data));
          this.tempProduct = response.data.data;

          console.log(this.tempProduct);
        })
        .catch((error) => {
          this.isLoading = false;
          console.log(error);
        });
    },
  },
  data() {
    return {
      isLoading: false,
      tempProduct: {
        imageUrl: [],
      },
    };
  },
};
</script>
