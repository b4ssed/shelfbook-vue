<template>
  <div class="container-fluid">
    <div class="row">
      <div class="col-lg-12">
        <div class="row mt-4">
          <div class="card min-height-400">
            <div class="card-body">
              <div class="row">
                <div class="col-md-2">
                  <img src="../assets/img/domcasmurro.jpg" alt="Image placeholder" class="card-img-top" width="250" height="400"/>
                </div>
                <div class="col-md-2">
                  <div class="mx-auto text-center mt-7">
                    <h5>
                      Mark Davis
                      <span class="font-weight-light">, 35</span>
                    </h5>
                    <div class="h6 font-weight-300">
                      <i class="ni location_pin mr-2"></i>Bucharest, Romania
                    </div>
                    <div class="h6 mt-4">
                      <i class="ni business_briefcase-24 mr-2"></i>Solution Manager - Creative Tim Officer
                    </div>
                    <div>
                      <i class="ni education_hat mr-2"></i>University of Computer Science
                    </div>
                  </div>
                </div>
              </div>
            </div>
          </div>  
        </div>
      </div>
    </div>
    <hr class="horizontal dark" />
    <div class="row">
      <div class="col-md-12">
        <billing-card />
      </div>
    </div>
    <div class="col-lg-4">
        <invoice-card class="mt-4" />
      </div>
  </div>
</template>

<script>
import InvoiceCard from "./components/InvoiceCard.vue";
import BillingCard from "./components/BillingCard.vue";


export default {
  name: "Billing",
  components: {
    InvoiceCard,
    BillingCard,
  },
  data() {
    return {
      salary: {
        classIcon: "text-white fas fa-landmark",
        title: "Salary",
        desc: "Belong Interactive",
        price: "+$2000",
      },
      paypal: {
        classIcon: "text-white fab fa-paypal",
        title: "Paypal",
        desc: "Freelance Payment",
        price: "$455.00",
      },
    };
  },
};
</script>
