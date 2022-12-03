<template>
  <div id="label">
    <el-row>
      <el-col :span="16">
        <div class="table-product">
          <el-row class="tb-product-header">
            <el-col :span="7">
              <el-select v-model="ChiNhanh.TenChiNhanh" placeholder="Chọn chi nhánh" style="width: 95%;">
                <el-option v-for="(item, index) in ChiNhanh" :key="index" :label="item.TenChiNhanh"
                  :value="item.TenChiNhanh"></el-option>
              </el-select>
            </el-col>
            <el-col :span="7">
              <el-select v-model="GiaBan.Loai" placeholder="Loại giá bán" style="width: 95%;">
                <el-option v-for="(item, index) in GiaBan" :key="index" :label="item.Loai" :value="item.Loai">
                </el-option>
              </el-select>
            </el-col>
            <el-col :span="8">
              <el-select v-model="Select" multiple filterable allow-create default-first-option
                placeholder="Chọn sản phẩm">
                <el-option v-for=" Product in Products" :key="Product.Hang" :label="Product.Hang"
                  :value="Product.Hang">
                </el-option>
              </el-select>
            </el-col>
            <el-col :span="2">
              <el-button @click="add" type="success">Thêm</el-button>
            </el-col>
          </el-row>
          <el-row>
            <el-table :data="ListProduct" height="310" style="width: 100%">
              <el-table-column prop="TenSp" label="Tên sản phẩm" width="180">
                <template slot-scope="scopeTen">
                  <el-input v-model="scopeTen.row.TenSp">
                  </el-input>
                </template>
              </el-table-column>
              <el-table-column prop="NSX" label="Ngày Sản xuất" width="160">
                <template slot-scope="scopeNSX">
                  <el-date-picker v-model="scopeNSX.row.NSX" type="date" value-format="dd/MM/yyyy"
                    placeholder="Chọn ngày" style="width: 150px;">
                  </el-date-picker>
                </template>
              </el-table-column>
              <el-table-column prop="HSD" label="Hạn sử dụng" width="160">
                <template slot-scope="scopeHSD">
                  <el-date-picker v-model="scopeHSD.row.HSD" type="date" value-format="dd/MM/yyyy"
                    placeholder="Chọn ngày" style="width: 150px;">
                  </el-date-picker>
                </template>
              </el-table-column>
              <el-table-column prop="ChiTiet" label="Chi tiết">
                <template slot-scope="scopeInput">
                  <el-input type="textarea" :rows="2" v-model="scopeInput.row.ChiTiet">
                  </el-input>
                </template>
              </el-table-column>
              <el-table-column prop="Sl" label="Số lượng" width="100">
                <template slot-scope="scopeSl">
                  <el-input v-model="scopeSl.row.Sl"></el-input>
                </template>
              </el-table-column>
              <el-table-column prop="Gia" label="Giá" width="100">
                <template slot-scope="scopeGia">
                  <el-input v-model="scopeGia.row.Gia"></el-input>
                </template>
              </el-table-column>
              <el-table-column width="80">
                <template slot-scope="scope">
                  <el-button @click.native.prevent="deleteRow(scope.$index, ListProduct)" type="danger" size="small">
                    Xóa
                  </el-button>
                </template>
              </el-table-column>
            </el-table>
          </el-row>
        </div>
        <div class="stamp-template">
          <el-row>
            <span class="template-header" style="background-color: white; width: 100%;">Mẫu in</span>
          </el-row>
          <el-divider></el-divider>
          <vue-html2pdf :show-layout="false" :float-layout="true" :enable-download="true" :preview-modal="true"
            :paginate-elements-by-height="1400" filename="label" :pdf-quality="2" :manual-pagination="false" pdf-format="legal"
            pdf-orientation="portrait" pdf-content-width="600px" ref="html2Pdf">
            <section slot="pdf-content">
              <div id="merchandise-sticker" style="display: flex;">
                <div class="inner-sticker"
                  style="font-size:10px;width:37mm;min-height:22mm;padding:1mm;margin:0;position:relative;border:1px dotted #f1f1f1;background-color: #fff; box-shadow: 0 0 5px #909399;">
                  <div class="sticker-title"><span>Tên SP: </span> {{ ListProduct[0].TenSp }} </div>
                  <div class="sticker-title"><span>NSX: </span> {{ ListProduct[0].NSX }} </div>
                  <div class="sticker-title"><span>HSD: </span> {{ ListProduct[0].HSD }} </div>
                  <div class="sticker-title"><span>Chi Tiết: </span> {{ ListProduct[0].ChiTiet }} </div>
                </div>
                <div class="inner-sticker"
                  style="font-size:10px;width:37mm;min-height:22mm;padding:1mm;margin:0;position:relative;border:1px dotted #f1f1f1;background-color: #fff; box-shadow: 0 0 5px #909399;">
                  <div class="sticker-title"><span>Tên SP: </span> {{ ListProduct[0].TenSp }} </div>
                  <div class="sticker-title"><span>NSX: </span> {{ ListProduct[0].NSX }} </div>
                  <div class="sticker-title"><span>HSD: </span> {{ ListProduct[0].HSD }} </div>
                  <div class="sticker-title"><span>Chi Tiết: </span> {{ ListProduct[0].ChiTiet }} </div>
                </div>
                <div v-if="PrintSize == 1" class="inner-sticker"
                  style="font-size:10px;width:37mm;min-height:22mm;padding:1mm;margin:0;position:relative;border:1px dotted #f1f1f1;background-color: #fff; box-shadow: 0 0 5px #909399;">
                  <div class="sticker-title"><span>Tên SP: </span> {{ ListProduct[0].TenSp }} </div>
                  <div class="sticker-title"><span>NSX: </span> {{ ListProduct[0].NSX }} </div>
                  <div class="sticker-title"><span>HSD: </span> {{ ListProduct[0].HSD }} </div>
                  <div class="sticker-title"><span>Chi Tiết: </span> {{ ListProduct[0].ChiTiet }} </div>
                </div>
              </div>
            </section>
          </vue-html2pdf>
          <el-row type="flex" justify="center" style="margin-top: 30px;">
          <div id="merchandise-sticker" style="display: flex;">
                <div class="inner-sticker"
                  style="font-size:16px;width:47mm;min-height:32mm;padding:2mm;margin:2px;position:relative;border:1px dotted #f1f1f1;background-color: #fff; box-shadow: 0 0 5px #909399;">
                  <div class="sticker-title"><span>Tên SP: </span> {{ ListProduct[0].TenSp }} </div>
                  <div class="sticker-title"><span>NSX: </span> {{ ListProduct[0].NSX }} </div>
                  <div class="sticker-title"><span>HSD: </span> {{ ListProduct[0].HSD }} </div>
                  <div class="sticker-title"><span>Chi Tiết: </span> {{ ListProduct[0].ChiTiet }} </div>
                </div>
                <div class="inner-sticker"
                  style="font-size:16px;width:47mm;min-height:32mm;padding:2mm;margin:2px;position:relative;border:1px dotted #f1f1f1;background-color: #fff; box-shadow: 0 0 5px #909399;">
                  <div class="sticker-title"><span>Tên SP: </span> {{ ListProduct[0].TenSp }} </div>
                  <div class="sticker-title"><span>NSX: </span> {{ ListProduct[0].NSX }} </div>
                  <div class="sticker-title"><span>HSD: </span> {{ ListProduct[0].HSD }} </div>
                  <div class="sticker-title"><span>Chi Tiết: </span> {{ ListProduct[0].ChiTiet }} </div>
                </div>
                <div v-if="PrintSize == 1" class="inner-sticker"
                  style="font-size:16px;width:47mm;min-height:32mm;padding:2mm;margin:2px;position:relative;border:1px dotted #f1f1f1;background-color: #fff; box-shadow: 0 0 5px #909399;">
                  <div class="sticker-title"><span>Tên SP: </span> {{ ListProduct[0].TenSp }} </div>
                  <div class="sticker-title"><span>NSX: </span> {{ ListProduct[0].NSX }} </div>
                  <div class="sticker-title"><span>HSD: </span> {{ ListProduct[0].HSD }} </div>
                  <div class="sticker-title"><span>Chi Tiết: </span> {{ ListProduct[0].ChiTiet }} </div>
                </div>
              </div>
            </el-row>
        </div>
      </el-col>
      <el-col :span="8">
        <div class="print-size">
          <el-row>
            <span class="template-header">Khổ in</span>
          </el-row>
          <el-divider></el-divider>
          <el-radio v-model="PrintSize" label="1">
            <span> Khổ 3</span>
            <el-image style="width: 200px; height: 200px; margin: 10px;"
              src="https://images.tcdn.com.br/img/img_prod/1041864/etiqueta_adesiva_papel_couche_33x21mm_3_carreiras_rolo_com_20m_e_2600_etiquetas_356961_1_f88ee61b233862942bf805683f3b0ec5.jpg"
              fit="cover">
            </el-image>
          </el-radio>
          <el-radio v-model="PrintSize" label="2">
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
      ListProduct: [
        { TenSp: '', NSX: '', HSD: '', ChiTiet: '', Sl: 0, Gia: 0 },
      ],
      Products: [
        { Hang: 'A', },
        { Hang: 'B', },
        { Hang: 'C', },
      ],
      Select: [],
      ChiNhanh: [
        { TenChiNhanh: 'Hồ Chí Minh' },
        { TenChiNhanh: 'Hà Nội' },
      ],
      GiaBan: [
        { Loai: 'Giá bán lẻ' },
        { Loai: 'Giá bán sỉ' },
      ],
      PrintSize: '2',
      htmlToPdfOptions: {
        jsPDF: {
          unit: 'in',
          format: '100px',
          orientation: 'portrait'
    }
}
    }
  },
  methods: {
    add() {
      var item = { TenSp: '', NSX: '', HSD: '', ChiTiet: '', Sl: 0, Gia: 0 };
      for (let i = 0; i < this.Select.length; i++) {
        this.ListProduct.push(cloneDeep(item));
      }
    },
    deleteRow(index, rows) {
      rows.splice(index, 1);
    },
    makePDF() {
      this.$refs.html2Pdf.generatePdf()
    },
  },
  components: {
    VueHtml2pdf
  },
}
</script>

<style lang="scss">
@import url('https://fonts.googleapis.com/css2?family=Montserrat:ital,wght@0,100;0,200;0,300;0,400;0,500;0,600;0,700;0,800;0,900;1,100;1,200;1,300;1,400;1,500;1,600;1,700;1,800;1,900&display=swap');

* {
  font-family: 'Montserrat', sans-serif;
  margin: 0;
  padding: 0;
}

#label {
  height: 100vh;
  background-color: #E4E7ED;

  .table-product {
    height: 380px;
    margin: 10px;
    background-color: #fff;
    box-shadow: 0 0 4px #DCDFE6;
    padding: 10px;
    border-radius: 8px;

    .tb-product-header {
      padding: 10px 20px;

      .el-select {
        width: 100%;
      }

      .el-button {
        margin-left: 10px;
      }
    }
  }

  .stamp-template {
    height: 290px;
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
    position: relative;
    height: 700px;
    margin: 10px;
    border-radius: 8px;
    background-color: #fff;
    box-shadow: 0 0 4px #DCDFE6;
    padding: 10px;

    .el-radio {
      margin: 10px;

      .el-radio__label {
        font-size: 16px;
      }
    }
  }
}
</style>
