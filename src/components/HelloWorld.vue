<template>
  <div class="eb-elx-proj">
    <div class="container-fluid">
      <div class="d-flex">
      <input
        type="file"
        class="form-control w-25"
        id="input"
        @change="elxHandler()"
      />
      <button class="btn btn-primary" @click="print">Print</button>
      </div>
      <div
        class="row m-2"
        id="printMe"
        style="
          display: -webkit-box;
          display: -ms-flexbox;
          display: flex;
          -ms-flex-wrap: wrap;
          flex-wrap: wrap;
          margin-right: -12px;
          margin-left: -12px;
        "
      >
      <template v-for="(dataobj, idx) in dataArray">
        <div :key="idx" :style="[idx%10 == 0 ? {marginTop: '100px'} : '',idx%10 == 1 ? {marginTop: '100px'} : '']"
            class="col-3 col-md-3 col-sm-12 col-lg-3 card eb-card"
            :class="[dataobj.name.length > 20 ? '' : 'mb-60']"
            style="
              -webkit-box-flex: 0;
              -ms-flex: 0 0 9.5cm;
              flex: 0 0 9.5cm;
              max-width: 9.5cm;
              background: #fff;
              border: 3px dotted #000;
              margin: 10px;
              min-height: 6.5cm !important;
              height: 6.5cm !important;
              max-height: 6.5cm !important;
              text-align: left !important;
              float: left;
              padding: 0px !important;
            ">
          <div
          >
            <div class="card-body">
              <h2
                class="font-size-25 eb-title"
                :class="[dataobj.name.length > 20 ? '' : 'mb-60']"
              >
                {{ dataobj.name }}
              </h2>
              <div class="mt-5" v-if="dataobj.detail.isOff">
                <h2 style="font-size: 70px; text-align: center">
                  {{ dataobj.detail.isOffPr }}%
                </h2>
              </div>
              <div
                v-else
                class="align-items-center d-flex justify-content-around mt-3"
              >
                <del style="font-size: 70px">{{ dataobj.detail.price }}</del>
                <span style="font-size: 70px">{{
                  dataobj.detail.offerPrice
                }}</span>
              </div>
            </div>
          </div>
        </div>
      </template>
      </div>
    </div>
  </div>
</template>

<script>
import readXlsxFile from "read-excel-file";

export default {
  name: "HelloWorld",
  data() {
    return {
      dataArray: [],
      output: null,
    };
  },
  methods: {
    elxHandler() {
      const input = document.getElementById("input");
      //      readXlsxFile(input.files[0]).then((rows) => {
      //   // `rows` is an array of rows
      //   // each row being an array of cells.
      //   console.log('data',JSON.stringify(rows));
      // })
      const map = {
        NAME: "name",
        PRODUCT: {
          detail: {
            IS_OFF: "isOff",
            IS_OFF_PR: "isOffPr",
            PRICE: "price",
            OFFER_PRICE: "offerPrice",
          },
        },
      };
      readXlsxFile(input.files[0], { map }).then(({ rows }) => {
        rows ===
          [
            {
              name: "NAME",
              detail: {
                isOff: "IS_OFF",
                isOffPr: "IS_OFF_PR",
                price: "PRICE",
                offerPrice: "OFFER_PRICE",
              },
            },
          ];
        this.dataArray = rows;
        console.log("data", rows);

        // genrate pdf
      });
    },
    print() {
      // Pass the element id here
      // this.$htmlToPaper('printMe');
      this.$htmlToPaper("printMe", null, () => {
        console.log("Printing completed or was cancelled!");
        // Window.print()
      });
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style media="print">
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
.font-size-25 {
  font-size: 25px !important;
}
.eb-card {
  min-height: 6.5cm !important;
  height: 6.5cm !important;
  max-height: 6.5cm !important;
  text-align: left !important;
  float: left;
}
.eb-card .eb-title {
  text-align: center;
  font-weight: 700;
}
.mb-60 {
  margin-bottom: 60px;
}
@page {
  size: landscape;
}
.page-break {
  display: block;
  page-break-before: always;
}
</style>
<style>
#printMe {
  display: none !important;
}
@media print {
  @page {
    size: landscape;
  }
}
</style>
