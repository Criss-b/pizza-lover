<template>
    <div class="bookContainer">
      <h1 class="title">Prenota</h1>
      <form class="form" @submit.prevent="booking">
        <fieldset>
          <img
            class="imgLeft1"
            src="@/assets/img/list.webp"
            alt="immagine lista"
          />
          <label class="label" for="name">Nome prenotazione</label>
          <input
            class="text styleBox"
            type="text"
            placeholder="Nome e cognome"
            v-model="name"
            id="name"
            required
            autocomplete="off"
          />
          <img class="imgRight1"
          src="@/assets/img/kawaii.webp"
          alt="immagine fetta di pizza"/>
        </fieldset>
  
        <fieldset>
          <label class="label" for="select">Pizza</label>
          <div class="selectPizze">
            <select
              class="selectPizza styleBox"
              :class="{ selectedPizza: pizzaSelect }"
              v-model="pizzaSelect"
              required
              id="select"
            >
              <option class="option" disabled value="">Seleziona...</option>
              <option
                class="option"
                v-for="pizza in listPizze"
                :key="pizza"
                :value="pizza"
                :selected="pizzaSelect"
              >
                <strong>{{ pizza.tasty }}</strong> ({{ pizza.ing }})
              </option>
              <option
                v-for="pizza in newPizze"
                :key="pizza.tasty"
                :value="pizza"
                :selected="pizzaSelect"
              >
                <strong>{{ pizza.tasty }}</strong> ({{ pizza.ing }})
              </option>
            </select>
            <bookingButton
              class="btn"
              @click.prevent="addPizza"
              :value="add"
            ></bookingButton>
            <bookingButton
              class="btn"
              @click.prevent="resetPizza"
              :value="remove"
            ></bookingButton>
          </div>
        </fieldset>
  
        <fieldset v-for="(select, index) in otherSelect" :key="index">
          <div class="selectPizze">
            <select
              class="selectPizza styleBox"
              :class="{ selectedPizza: select.otherPizza }"
              v-model="select.otherPizza"
              :id="index"
              required
            >
              <option class="option" value="" disabled>Seleziona...</option>
              <option
                class="option"
                v-for="pizza in listPizze"
                :key="pizza"
                :value="pizza"
              >
                <strong>{{ pizza.tasty }}</strong> ({{ pizza.ing }})
              </option>
              <option v-for="pizza in newPizze" :key="pizza.tasty" :value="pizza">
                <strong>{{ pizza.tasty }}</strong> ({{ pizza.ing }})
              </option></select
            ><bookingButton
              class="btn"
              @click.prevent="addPizza"
              :value="add"
            ></bookingButton>
            <bookingButton
              class="btn"
              @click.prevent="removePizza"
              :value="remove"
            ></bookingButton>
          </div>
        </fieldset>
  
        <fieldset>
          <img class="imgLeft2"
          src="@/assets/img/kawaii.webp"
          alt="immagine fetta di pizza"/>
          <label class="label" for="notes">Note</label>
          <textarea
            class="textarea styleBox"
            placeholder="Specificare se allergie o se aggiungere ingrediente"
            v-model="notes"
            id="notes"
          ></textarea>
          <img
            class="imgRight2"
            src="@/assets/img/list.webp"
            alt="immagine lista"
          />
        </fieldset>
        <bookingButton
          class="booking"
          type="submit"
          :value="book"
        ></bookingButton>
      </form>
    </div>
  </template>
  
  <script>
  import bookingButton from "@/components/bookingButton.vue";
  import { mapState } from "vuex";
  export default {
    name: "bookPage",
    components: {
      bookingButton,
    },
    data() {
      return {
        name: "",
        number: "",
        notes: "",
        add: "➕",
        remove: "🗑️",
        book: "PRENOTA",
        otherSelect: [],
        pizzaSelect: "",
        listPizze: [
          {
            tasty: "Margherita",
            ing: "impasto classico, pomodoro, mozzarella",
            price: "5.00",
          },
          {
            tasty: "Marinara",
            ing: "impasto classico, pomodoro, aglio e origano",
            price: "5.00",
          },
          {
            tasty: "Diavola",
            ing: "impasto classico, pomodoro, mozzarella e salame piccante",
            price: "6.00",
          },
          {
            tasty: "Quattro formaggi",
            ing: "impasto classico, mozzarella, parmigiano,gorgonzola e provola",
            price: "6.50",
          },
          {
            tasty: "Quattro stagioni",
            ing: "impasto classico, pomodoro, mozzarella, carciofi, prosciutto cotto, funghi e olive",
            price: "7.00",
          },
          {
            tasty: "Capricciosa",
            ing: "impasto classico, pomodoro, mozzarella, carciofi, prosciutto cotto, funghi e olive",
            price: "6.00",
          },
          {
            tasty: "Romana",
            ing: "impasto classico, pomodoro, mozzarella, acciughe e origano",
            price: "6.50",
          },
          {
            tasty: "Funghi",
            ing: "impasto classico, pomodoro, mozzarella e funghi trifolati",
            price: "6.50",
          },
          {
            tasty: "Salsiccia",
            ing: "impasto classico, pomodoro, mozzarella e salsiccia",
            price: "6.00",
          },
          {
            tasty: "Salsiccia e friarielli",
            ing: "impasto classico, pomodoro, mozzarella e salsiccia e friarielli",
            price: "6.50",
          },
          {
            tasty: "Salsiccia e patate",
            ing: "impasto classico, mozzarella, salsiccia e patate",
            price: "6.50",
          },
          {
            tasty: "Frutti di mare",
            ing: "impasto classico, pomodoro, mozzarella e frutti di mare",
            price: "7.50",
          },
          {
            tasty: "Mari e monti",
            ing: "impasto classico, pomodoro, mozzarella, prosciutto cotto e funghi e con misto di mare",
            price: "8.00",
          },
          {
            tasty: "Vegetariana",
            ing: "impasto classico, pomodoro, mozzarella, zucchine, peperoni e melanzane grigliate",
            price: "6.50",
          },
          {
            tasty: "Valtellinese",
            ing: "impasto classico, pomodoro, mozzarella, bresaola, rucola e grana",
            price: "7.00",
          },
          {
            tasty: "Al salmone",
            ing: "impasto classico, pomodoro, mozzarella, panna e salmone",
            price: "7.00",
          },
        ],
        selectedPizza: [],
        totalPrice: [],
        whatsappUrl: "",
        summary: 0.0,
        firstChoice: "",
        otherChoice: "",
      };
    },
    computed: {
      ...mapState(["newPizze"]),
    },
    methods: {
      addPizza() {
        this.otherSelect.push({ otherPizza: "" });
      },
      resetPizza() {
        this.pizzaSelect = "";
      },
      removePizza(index) {
        this.otherSelect.splice(
          this.otherSelect.findIndex(({ id }) => id === index),
          1
        );
  
        this.otherSelect = this.otherSelect.filter((id) => id != index);
      },
      booking() {
        const phone = ""; // no number, choose the contact you want 
        this.firstChoice =
          this.pizzaSelect.tasty + " (" + this.pizzaSelect.ing + ")";
        const totalPartial = Number(this.pizzaSelect.price);
        this.totalPrice.push(totalPartial);
  
        this.otherSelect.forEach((select) => {
          const selectedPizza =
            select.otherPizza.tasty + " (" + select.otherPizza.ing + ")";
          this.selectedPizza.push(selectedPizza);
  
          const totalPartial = Number(select.otherPizza.price);
          this.totalPrice.push(totalPartial);
        });
  
        if (this.selectedPizza.length == 0) {
          this.otherChoice = "";
        } else {
          this.firstChoice = this.firstChoice + ", ";
          this.otherChoice = this.selectedPizza.join(", ");
        }
        if (this.notes == "") {
          this.notes = "Nessuna";
        } else {
          this.notes;
        }
  
        this.summary = this.totalPrice.reduce((acc, item) => acc + item, 0);
        const formattedSummary = this.summary.toLocaleString("it-IT", {
          style: "currency",
          currency: "EUR",
        });
  
        const iva = this.summary * 0.22;
        const formattedIva = iva.toLocaleString("it-IT", {
          style: "currency",
          currency: "EUR",
        });
  
        this.message =`Prenotazione a nome: ${this.name}
Pizza:
${this.firstChoice}
${this.otherChoice}
Note: ${this.notes}
  
Totale: ${formattedSummary}
(di cui IVA ${formattedIva})`;
        const resp = window.confirm(
          `Procedere con l'invio del messaggio?
${this.message}`
        );
        if (resp) {
          const url =
            "https://api.whatsapp.com/send?phone=" +
            phone +
            "&text=" +
            encodeURIComponent(this.message);
          window.open(url);
          window.location.reload();
        } else {
          this.selectedPizza = [];
          this.totalPrice = [];
        }
      },
    },
  };
  </script>
  
  <style lang="scss" scoped>
  .bookContainer {
    background-color: #282525;
    min-height: 100vh;
    padding-bottom: 90px;
    color: white;
    padding-top: 30px;
  }
  .title {
    text-align: center;
    color: white;
  }
  .form {
    display: flex;
    flex-direction: column;
    align-items: center;
  }

  .text {
    background-color: #3b3838;
    color: white;
    margin: 2px;
  }
  .selectPizze {
    display: flex;
    flex-direction: row;
    align-items: center;
    justify-content: space-evenly;
  }
  .selectPizza {
    background-color: #3b3838;
    color: rgb(133, 133, 133);
    width: 80%;
  }
  .selectPizza:hover {
    cursor: pointer;
  }
  .selectPizza option {
    background-color: #3b3838;
    color: white;
  }
  .selectedPizza {
    background-color: #3b3838;
    color: white;
  }
  .btn {
    height: 25px;
    width: 8%;
    font-size: 12px;
  }
  .textarea {
    background-color: #3b3838;
    color: white;
    border: none;
    margin: 2px;
    height: 100px;
    padding-top: 5px;
    resize: none;
  }
  .booking {
    height: 25px;
    width: 10%;
    font-size: 17px;
  }
  .imgLeft1 , .imgRight1 , .imgLeft2 , .imgRight2 {
    position: absolute;
  }
  .imgLeft1 {
    top: -30px;
    left: -250px;
  }
  .imgRight1{
    right: -300px;
  }
  .imgLeft2 {
    left: -350px;
    top: 60px;
  }
  .imgRight2 {
    bottom: -30px;
    right: -250px;
  }
  .imgLeft1 , .imgRight2 {
    height: 50px;
    width: 50px;
    transform: rotate(25deg);
  }
  .imgRight1 , .imgLeft2 {
    transform: rotate(335deg);
    height: 80px;
    width: 80px;
  }
  @media only screen and (max-width: 376px) {
    .title {
      font-size: large;
    }
    .label {
      font-size: medium;
    }
    .styleBox {
      height: 50px;
      padding-left: 0;
      font-size: 15px;
    }
    .text {
      padding-left: 5px;
    }
    .selectPizza {
      width: 70%;
    }
    .btn {
      font-size: 12px;
      height: 40px;
      width: 10%;
      padding: 0;
    }
  
    .textarea {
      margin: 2px;
      height: 90px;
      width: 200px;
      padding-left: 5px;
    }
    .booking {
      height: 25px;
      width: 25%;
      font-size: 12px;
    }
    .imgLeft1 ,.imgRight1 ,.imgLeft2 ,.imgRight2 {
      display: none;
    }
  }
  @media only screen and (min-width: 377px) and (max-width: 426px) {
    .title {
      font-size: large;
    }
    .label {
      font-size: medium;
    }
    .styleBox {
      height: 50px;
      padding-left: 0;
      font-size: 15px;
    }
    .text {
      padding-left: 5px;
    }
    .selectPizza {
      width: 70%;
    }
    .btn {
      height: 40px;
      width: 10%;
      font-size: 12px;
    }
    .textarea {
      margin: 2px;
      height: 90px;
      width: 200px;
      padding-left: 5px;
    }
    .booking {
      height: 25px;
      width: 20%;
      font-size: 12px;
    }
    .imgLeft1 ,.imgRight1 , .imgLeft2 , .imgRight2 {
      display: none;
    }
  }
  @media only screen and (min-width: 427px) and (max-width: 675px) {
    .title {
      font-size: large;
    }
    .label {
      font-size: small;
    }
    .styleBox {
      height: 40px;
      padding-left: 0;
      font-size: 12px;
    }
    .text {
      padding-left: 5px;
    }
    .selectPizza {
      width: 70%;
    }
    .btn {
      height: 25px;
      width: 10%;
      font-size: 12px;
    }
    .textarea {
      margin: 2px;
      height: 50px;
      width: 250px;
      padding-left: 5px;
    }
    .booking {
      height: 25px;
      width: 20%;
      font-size: 12px;
    }
    .imgLeft1 {
      top: -10%;
      left: -20%;
    }
    .imgRight1 {
      right: -25%;
    }
    .imgLeft2 {
      left: -18%;
      top: 15%;
    }
    .imgRight2 {
      bottom: -10%;
      right: -18%;
    }
    .imgLeft1 , .imgRight2 {
      height: 40px;
      width: 40px;
    }
    .imgRight1 , .imgLeft2 {
      height: 55px;
      width: 55px;
    }
  }
  @media only screen and (min-width: 676px) and (max-width: 1024px) {
    .booking {
      height: 25px;
      width: 20%;
      font-size: 15px;
    }
    .imgLeft1 {
      top: -10%;
      left: -25%;
    }
    .imgRight1 {
      top: 50%;
      right: -25%;
    }
    .imgLeft2 {
      left: -45%;
      top: 25%;
    }
    .imgRight2 {
      bottom: 15%;
      right: -45%;
    }
    .imgLeft1 , .imgRight2 {
      height: 40px;
      width: 40px;
    }
    .imgRight1 , .imgLeft2 {
      height: 55px;
      width: 55px;
    }
  }
  </style>
  