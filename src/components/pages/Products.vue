<template>
    <div>
        <div class="text-right mt-4">
            <button type="button" class="btn btn-primary" @click="openModal(true)">
                建立新產品
            </button>
            
        </div>
        <table class="table mt-4">
            <thead>
                <th width="120">分類</th>
                <th>產品名稱</th>
                <th width="120">原價</th>
                <th width="120">售價</th>
                <th width="100">是否啟用</th>
                <th width="80">編輯</th>
            </thead>
            <tbody>
                <tr v-for="(item, key) in products" :key="key">
                    <td>{{item.category}}</td>
                    <td>{{item.title}}</td>
                    <td class="text-right">
                    {{item.origin_price}}
                    </td>
                    <td class="text-right">
                    {{item.price}}
                    </td>
                    <td>
                        <span v-if="item.is_enabled" class="text-success">啟用</span>
                        <span>未啟用</span>
                    </td>
                    <td>
                        <button class="btn btn-outline-primary btn-sm" @click="openModal(false, item)">編輯</button>
                        <button class="btn btn-outline-danger btn-sm" @click="openDelModal(item)">刪除</button>
                    </td>
                </tr>
            </tbody>
        </table>
        <div class="modal fade" id="productModal" tabindex="-1" role="dialog" aria-labelledby="exampleModalLabel" aria-hidden="true">
            <div class="modal-dialog modal-lg" role="document">
                <div class="modal-content border-0">
                <div class="modal-header bg-dark text-white">
                    <h5 class="modal-title" id="exampleModalLabel">
                    <span>新增產品</span>
                    </h5>
                    <button type="button" class="close" data-dismiss="modal" aria-label="Close">
                    <span aria-hidden="true">&times;</span>
                    </button>
                </div>
                <div class="modal-body">
                    <div class="row">
                    <div class="col-sm-4">
                        <div class="form-group">
                        <label for="image">輸入圖片網址</label>
                        <input type="text" class="form-control" id="image"
                            placeholder="請輸入圖片連結" v-model="tempProduct.imageUrl">
                        </div>
                        <div class="form-group">
                        <label for="customFile">或 上傳圖片
                            <i class="fas fa-spinner fa-spin"></i>
                        </label>
                        <input type="file" id="customFile" class="form-control"
                            ref="files">
                        </div>
                        <img img="https://images.unsplash.com/photo-1483985988355-763728e1935b?ixlib=rb-0.3.5&ixid=eyJhcHBfaWQiOjEyMDd9&s=828346ed697837ce808cae68d3ddc3cf&auto=format&fit=crop&w=1350&q=80"
                        class="img-fluid" alt="">
                    </div>
                    <div class="col-sm-8">
                        <div class="form-group">
                        <label for="title">標題</label>
                        <input type="text" class="form-control" id="title"
                            placeholder="請輸入標題" v-model="tempProduct.title">
                        </div>

                        <div class="form-row">
                        <div class="form-group col-md-6">
                            <label for="category">分類</label>
                            <input type="text" class="form-control" id="category"
                            placeholder="請輸入分類" v-model="tempProduct.category">
                        </div>
                        <div class="form-group col-md-6">
                            <label for="price">單位</label>
                            <input type="unit" class="form-control" id="unit"
                            placeholder="請輸入單位" v-model="tempProduct.unit">
                        </div>
                        </div>

                        <div class="form-row">
                        <div class="form-group col-md-6">
                        <label for="origin_price">原價</label>
                            <input type="number" class="form-control" id="origin_price"
                            placeholder="請輸入原價" v-model="tempProduct.origin_price">
                        </div>
                        <div class="form-group col-md-6">
                            <label for="price">售價</label>
                            <input type="number" class="form-control" id="price"
                            placeholder="請輸入售價" v-model="tempProduct.price">
                        </div>
                        </div>
                        <hr>

                        <div class="form-group">
                        <label for="description">產品描述</label>
                        <textarea type="text" class="form-control" id="description"
                            placeholder="請輸入產品描述" v-model="tempProduct.description"></textarea>
                        </div>
                        <div class="form-group">
                        <label for="content">說明內容</label>
                        <textarea type="text" class="form-control" id="content"
                            placeholder="請輸入產品說明內容" v-model="tempProduct.content"></textarea>
                        </div>
                        <div class="form-group">
                        <div class="form-check">
                            <input class="form-check-input" type="checkbox"
                            id="is_enabled" v-model="tempProduct.is_enabled" :true-value="1" :false-value="0">
                            <label class="form-check-label" for="is_enabled">
                            是否啟用
                            </label>
                        </div>
                        </div>
                    </div>
                    </div>
                </div>
                <div class="modal-footer">
                    <button type="button" class="btn btn-outline-secondary" data-dismiss="modal">取消</button>
                    <button type="button" class="btn btn-primary" @click="updateproduct">確認</button>
                </div>
                </div>
            </div>
        </div>
        <div class="modal fade" id="delProductModal" tabindex="-1" role="dialog"
            aria-labelledby="exampleModalLabel" aria-hidden="true">
            <div class="modal-dialog" role="document">
                <div class="modal-content border-0">
                <div class="modal-header bg-danger text-white">
                    <h5 class="modal-title" id="exampleModalLabel">
                    <span>刪除產品</span>
                    </h5>
                    <button type="button" class="close" data-dismiss="modal" aria-label="Close">
                    <span aria-hidden="true">&times;</span>
                    </button>
                </div>
                <div class="modal-body">
                    是否刪除 <strong class="text-danger">{{ tempProduct.title }}</strong> 商品(刪除後將無法恢復)。
                </div>
                <div class="modal-footer">
                    <button type="button" class="btn btn-outline-secondary" data-dismiss="modal">取消</button>
                    <button type="button" class="btn btn-danger" @click="Delproduct">確認刪除</button>
                </div>
                </div>
            </div>
        </div>

        
    </div>
</template>

<script>
    import $ from 'jquery';
    export default{
        data(){
            return {
                products:[],
                tempProduct:{},
                isNew: false,
            }
        },
        methods:{
            getProducts(){
                const vm = this;
                const api = `${process.env.APIPATH}/api/${process.env.CUSTOMPATH}/admin/products/all`;
                this.$http.get(api).then((response) => {
                    console.log(response.data);
                    vm.products = response.data.products;
                })
            },
            openModal(isNew, item){
                $('#productModal').modal('show');
                if(isNew){
                    this.tempProduct = {};
                    this.isNew = true;
                } else{
                    this.tempProduct = Object.assign({}, item);
                    this.isNew = false;
                } 
            },
            openDelModal(item){
                $('#delProductModal').modal('show');
                this.tempProduct = Object.assign({}, item); 
            },
            Delproduct(){
                const vm = this;
                let api = `${process.env.APIPATH}/api/${process.env.CUSTOMPATH}/admin/product/${vm.tempProduct.id}`
                this.$http.delete(api).then((response) => {
                    if(response.data.success){
                        console.log('刪除成功');
                        $('#delProductModal').modal('hide');
                        vm.getProducts();
                    } else{
                        console.log('刪除失敗');
                        $('#delProductModal').modal('hide');
                        vm.getProducts();
                    } 
                })
            },
            updateproduct(){
                const vm = this;
                let api = `${process.env.APIPATH}/api/${process.env.CUSTOMPATH}/admin/product`;
                let httpMethod = 'post';
                
                if(!vm.isNew){
                    console.log('id',vm.tempProduct.id);
                    api = `${process.env.APIPATH}/api/${process.env.CUSTOMPATH}/admin/product/${vm.tempProduct.id}`;
                    httpMethod = 'put';
                }
                this.$http[httpMethod](api, {data: vm.tempProduct}).then((response) => {
                    console.log(response.data);
                    if(response.data.success){
                        $('#productModal').modal('hide');
                        vm.getProducts();
                        
                    } else {
                        $('#productModal').modal('hide');
                        vm.getProducts();
                        console.log('新增產品失敗');
                    }
                })
            }
        },
        created(){
            this.getProducts()
        }
    }
    
</script>