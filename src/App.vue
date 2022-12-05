<template>
  <div id="label">
    <el-row>
      <el-col :span="16">
        <div class="table-product">
          <el-row class="tb-product-header">
            <el-col>
              <el-button @click="addRow" type="success">Thêm dòng</el-button>
              <el-button @click="diviceForRows" type="danger">Xem trước</el-button>
            </el-col>
          </el-row>
          <el-row>
            <el-table :data="productList" height="310" style="width: 100%">
              <el-table-column prop="name" label="Tên sản phẩm" width="180">
                <template slot-scope="data">
                  <el-input v-model="data.row.name">
                  </el-input>
                </template>
              </el-table-column>
              <el-table-column prop="createdDate" label="Ngày Sản xuất" width="160">
                <template slot-scope="data">
                  <el-date-picker v-model="data.row.createdDate" type="date" value-format="dd/MM/yyyy"
                    placeholder="Chọn ngày" style="width: 150px;">
                  </el-date-picker>
                </template>
              </el-table-column>
              <el-table-column prop="expiredAt" label="Hạn sử dụng" width="160">
                <template slot-scope="data">
                  <el-date-picker v-model="data.row.expiredAt" type="date" value-format="dd/MM/yyyy"
                    placeholder="Chọn ngày" style="width: 150px;">
                  </el-date-picker>
                </template>
              </el-table-column>
              <el-table-column prop="note" label="Chi tiết">
                <template slot-scope="data">
                  <el-input type="textarea" :rows="2" v-model="data.row.note">
                  </el-input>
                </template>
              </el-table-column>
              <el-table-column prop="quantity" label="Số lượng" width="100">
                <template slot-scope="data">
                  <el-input v-model="data.row.quantity"></el-input>
                </template>
              </el-table-column>
              <el-table-column prop="price" label="Giá" width="100">
                <template slot-scope="data">
                  <el-input v-model="data.row.price"></el-input>
                </template>
              </el-table-column>
              <el-table-column width="80">
                <template slot-scope="scope">
                  <el-button @click.native.prevent="deleteRow(scope.$index, productList)" type="danger" size="small">
                    Xóa
                  </el-button>
                </template>
              </el-table-column>
            </el-table>
          </el-row>
        </div>
        <div class="label-template">
          <el-row>
            <span class="template-header" style="background-color: white; width: 100%;">Mẫu in</span>
          </el-row>
          <el-divider></el-divider>
          <div style="display: flex;justify-content: center;margin-top: 20px;">
          <div id="print-pdf">
            <div id="row-sticker" v-for="(page, index) in productRows" :key="index" style="page-break-after: always;">
                  <div class="inner-sticker"
                    v-for="(product, index) in page" :key="index"
                    style="display:inline-block;font-size:10px;width:37mm;min-height:22mm;padding:1mm;margin:0;position:relative;border:1px dotted #f1f1f1;background-color: #fff; box-shadow: 0 0 1px #909399;">
                    <div class="sticker-title"><span>Tên SP: </span> {{ product.name }} </div>
                    <div class="sticker-title"><span>NSX: </span> {{ product.createdDate }} </div>
                    <div class="sticker-title"><span>HSD: </span> {{ product.expiredAt }} </div>
                    <div class="sticker-title"><span>Chi Tiết: </span> {{ product.note }} </div>
                  </div>
            </div>
        </div>
        </div>
        </div>
      </el-col>
      <el-col :span="8">
        <div class="print-size">
          <el-row>
            <span class="template-header">Khổ in</span>
          </el-row>
          <el-divider></el-divider>
          <el-radio v-model="quantityPerRow" :label="3">
            <span> Khổ 3</span>
            <el-image style="width: 200px; height: 200px; margin: 10px;"
              src="https://images.tcdn.com.br/img/img_prod/1041864/etiqueta_adesiva_papel_couche_33x21mm_3_carreiras_rolo_com_20m_e_2600_etiquetas_356961_1_f88ee61b233862942bf805683f3b0ec5.jpg"
              fit="cover">
            </el-image>
          </el-radio>
          <el-radio v-model="quantityPerRow" :label="2">
            <span> Khổ 2</span>
            <el-image style="width: 200px; height: 200px; margin-top: 20px;"
              src="https://giayinnhiet.com.vn/images/product/1615802075cR5YILivde.jpeg" fit="cover">
            </el-image>
          </el-radio>
          <el-button @click="makePDF"
            style="margin-top: 40px; margin-right: 10px; position: absolute; bottom: 10px; right: 10px; " type="danger">
            Xuất file PDF</el-button>
        </div>
      </el-col>
    </el-row>
  </div>
</template>
<script>
import { cloneDeep } from 'lodash';
import VueHtml2pdf from 'vue-html2pdf'
export default {
  data() {
    return {
      productAttributes: {
        name: '',
        createdDate: '',
        expiredAt: '',
        note: '',
        quantity: 1,
        price: 0
      },
      productList: [],
      quantityPerRow: 2,
      productRows: [],
    }
  },

  mounted() {
    this.addRow()
    this.addRow()
  },  
  methods: {
    addRow() {
      this.productList.push(cloneDeep(this.productAttributes));
    },
    deleteRow(index, rows) {
      rows.splice(index, 1);
    },
    makePDF() {
      var prtContent = document.getElementById("print-pdf");
      var WinPrint = window.open();
      WinPrint.document.write(prtContent.innerHTML);
      WinPrint.document.close();
      WinPrint.print();
    },
    diviceForRows() {
      let productRows = []
      console.log(this.quantityPerRow)
      let totalProductLabels = this.productList.reduce((total, item) => {
        return total += parseInt(item.quantity)
      }, 0)
      let totalRows = Math.ceil(totalProductLabels / this.quantityPerRow)
      let rawListProductByItem = []
      this.productList.forEach(product => {
        for (let i = 0; i < product.quantity; i++) {
          rawListProductByItem.push(cloneDeep(product))
        }
      })

      let quantityPerRow = this.quantityPerRow
      let listProductByItem = cloneDeep(rawListProductByItem)
      for(let i = 0; i < totalRows; i++ ) {
        let productRow = []
        for (let y = 0; y < quantityPerRow; y++) {
          if (!listProductByItem[y]) break;
          productRow.push(cloneDeep(listProductByItem[y]));
        }
        listProductByItem.splice(0, 2)
        productRows.push([...productRow])
      }
      this.productRows = productRows
      console.log(this.productRows)
    },

    reset() {

    }
  },
  components: {
    VueHtml2pdf
  },
}
</script>

<style  lang="scss">
@import url('https://fonts.googleapis.com/css2?family=Montserrat:ital,wght@0,100;0,200;0,300;0,400;0,500;0,600;0,700;0,800;0,900;1,100;1,200;1,300;1,400;1,500;1,600;1,700;1,800;1,900&display=swap');
* {
  font-family: 'Montserrat', sans-serif;
  margin: 0;
  padding: 0;
}
#label {
  height: auto;
  background-color: #E4E7ED;
  position: relative;

  .table-product {
    height: 380px;
    margin: 10px;
    background-color: #fff;
    box-shadow: 0 0 4px #DCDFE6;
    padding: 10px;
    border-radius: 8px;

    .tb-product-header {
      .el-select {
        width: 100%;
      }

      .el-button {
        margin-left: 10px;
      }
    }
  }

  .label-template {
    min-height: 290px;
    margin: 10px;
    border-radius: 8px;
    background-color: #fff;
    box-shadow: 0 0 4px #DCDFE6;
    padding: 10px;

    .sticker-title {
      padding: 0.6mm;
      font-weight: 600;
      span {
        font-weight: 400;
      }
    }
  }
  .el-divider.el-divider--horizontal {
    margin: 10px 0;
  }

  .template-header {
    margin: 10px;
  }

  .print-size {
    position: absolute;
    min-height: 700px;
    margin: 10px;
    border-radius: 8px;
    background-color: #fff;
    box-shadow: 0 0 4px #DCDFE6;
    padding: 10px;
    bottom: 0;
    top: 0;

    .el-radio {
      margin: 10px;

      .el-radio__label {
        font-size: 16px;
      }
    }
  }
}
</style>
