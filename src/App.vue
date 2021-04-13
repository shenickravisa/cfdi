<template>
  <v-app>
    <v-container>
      <v-layout row wrap>
        <v-flex xs12 md6>
          <div class="give-space-field-right">
            <v-text-field
              label="Filtro"
              center-line
              hide-details
            ></v-text-field>
          </div>
        </v-flex>

        <v-flex xs12 md6>
          <div class="give-space-field-left">
            <v-select
              v-model="cfdiFilterChoosed"
              :items="cfditype"
              label="Escoge un filtro"
              @change="onFilterSelected"
            ></v-select>
          </div>
        </v-flex>

        <v-flex xs12>
          <v-data-table
            :headers="headers"
            :items="cfdireport"
            :items-per-page="5"
            :pagination.sync="pagination"
            :total-items="totalElements"
          >
          </v-data-table>
        </v-flex>
      </v-layout>
    </v-container>
  </v-app>
</template>

<style lang="scss">
.give-space-field-right {
  margin-right: 20px;
  @media (max-width: 960px) {
    margin: 0px 10px 0px 10px;
  }
}
.give-space-field-left {
  margin-left: 20px;
  @media (max-width: 960px) {
    margin: 0px 10px 0px 10px;
  }
}
</style>


<script>
import cfdiservices from "@/services/cfdi";
// import cfdiFixtures from "@/fixtures/cfdiFixtures";

export default {
  name: "App",

  components: {},

  data: () => ({
    filter: {
      search: "",
      added_by: "",
    },
    cfdiFilterChoosed: "",
    cfditype: ["Ingresos", "Egresos"],
    headers: [
      {
        text: "Status",
        align: "start",
        sortable: true,
        value: "status",
      },
      { text: "Fecha de Cancelación", value: "cancellation_datetime" },
      { text: "Versión", value: "version" },
      { text: "Tipo de CFDI", value: "cfdi_type" },
      { text: "Series", value: "series" },
      { text: "Folio", align: "center", value: "folio" },
      { text: "CFDI", align: "rigth", value: "uuid" },
      { text: "Emisión", value: "cfdi_date_time_t" },
      { text: "Receptor", value: "receiver_rfc" },
      { text: "Descripción del Receptor", value: "receiver_name" },
      { text: "Emisor", value: "issuer_rfc" },
      { text: "Descripción Emisor", value: "issuer_name" },
      { text: "Descripcion", value: "descripcion" },
      { text: "Metodo de Pago", value: "payment_method" },
      { text: "Forma de Pago", value: "payment_mode" },
      { text: "SubTotal", value: "subtotal" },
      { text: "Descuento", value: "discount_total_amount" },
      { text: "Impuestos", value: "taxes" },
      { text: "IVA Retenido", value: "iva_retenido" },
      { text: "Total", value: "total" },
    ],
    cfdireport: [],
  }),
  created() {
    this.getcfidireport();
    // this.cfdireport = cfdiFixtures;
  },
  methods: {
    async getcfidireport() {
      this.cfdireport = await cfdiservices.getcfdireporissues();
    },

    onFilterSelected() {
      this.getcfidireport();
      let cfdiTemporal = this.cfdireport;
      if (this.cfdiFilterChoosed === "Ingresos") {
        this.cfdireport = cfdiTemporal.filter(
          (cdfiReport) => cdfiReport.cfdi_type === "Ingresos"
        );
      } else {
        this.cfdireport = cfdiTemporal.filter(
          (cdfiReport) => cdfiReport.cfdi_type === "Egresos"
        );
      }
    },
  },
};
</script>
