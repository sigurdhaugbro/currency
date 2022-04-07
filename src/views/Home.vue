<template>
   <main class="converter">
      <svg class="converter__logo" width="245" height="245" viewBox="0 0 245 245" fill="none" xmlns="http://www.w3.org/2000/svg">
         <path d="M153.125 188.854C140.99 188.865 129.085 185.538 118.714 179.236C108.343 172.934 99.9058 163.901 94.325 153.125H142.917C148.531 153.125 153.125 148.531 153.125 142.917C153.125 137.302 148.531 132.708 142.917 132.708H87.5875C87.0771 129.34 86.7708 125.971 86.7708 122.5C86.7708 119.029 87.0771 115.66 87.5875 112.292H142.917C148.531 112.292 153.125 107.698 153.125 102.083C153.125 96.4687 148.531 91.875 142.917 91.875H94.325C99.9157 81.1071 108.355 72.0808 118.724 65.7804C129.093 59.48 140.992 56.1474 153.125 56.1458C165.885 56.1458 177.829 59.8208 188.038 66.0479C193.142 69.2125 199.777 68.7021 204.065 64.4146C209.985 58.4937 208.658 48.7958 201.513 44.4062C186.972 35.4166 170.22 30.6453 153.125 30.625C113.108 30.625 79.2167 56.2479 66.5583 91.875H40.8333C35.2188 91.875 30.625 96.4687 30.625 102.083C30.625 107.698 35.2188 112.292 40.8333 112.292H61.8625C61.0357 119.072 61.0357 125.928 61.8625 132.708H40.8333C35.2188 132.708 30.625 137.302 30.625 142.917C30.625 148.531 35.2188 153.125 40.8333 153.125H66.5583C79.2167 188.752 113.108 214.375 153.125 214.375C170.888 214.375 187.425 209.373 201.513 200.594C208.556 196.204 209.883 186.404 203.963 180.483C199.675 176.196 193.04 175.685 187.935 178.952C177.829 185.281 165.988 188.854 153.125 188.854Z" fill="black"/>
      </svg>
      <input v-if="togleEur" v-model="inputValue" @keyup.enter="fetchNew" class="converter__input" type="numbers" placeholder="EU">
      <input v-if="togleNok" v-model="inputValue" @keyup.enter="convertNok" class="converter__input" type="numbers" placeholder="NOK">
      <button v-if="togleEur" @click="fetchNew" class="converter__equal">=</button>
      <button v-if="togleNok" @click="convertNok" class="converter__equal">=</button>
      <!-- v-if skjuler eller viser nok til eur eller eur til nok -->
      <div class="converter__output"> {{ outputValue }}</div>
      <div class="converter__buttons">
         <button @click="togleEur ^= true; togleNok ^= true" class="converter__swap">
            <svg width="50" height="50" viewBox="0 0 50 50" fill="none" xmlns="http://www.w3.org/2000/svg">
               <path d="M37.5 4.16663L43.75 10.4166L37.5 16.6666" stroke="black" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"/>
               <path d="M12.5 45.8333L6.25 39.5833L12.5 33.3333" stroke="black" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"/>
               <path d="M43.75 10.4166H20.8333C16.9656 10.4166 13.2563 11.9531 10.5214 14.688C7.78645 17.4229 6.25 21.1322 6.25 25" stroke="black" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"/>
               <path d="M6.25 39.5833H29.1667C33.0344 39.5833 36.7437 38.0469 39.4786 35.312C42.2135 32.5771 43.75 28.8677 43.75 25" stroke="black" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"/>
            </svg>
         </button>
      </div>
   </main>
   <nav class="converter__navigation">
      <div class="navigation__button"> {{ multipleCurrency }}</div>
   </nav>
</template>

<style>
   .converter {
      width: 100vw;
      height: 100vh;
      display: grid;
      justify-content: center;
      padding-top: 85px;
      position: relative;
   }

   .converter__input {
      width: 272px;
      height: 68px;
      background: var(--main-color);
      border: none;
      font-family: monospace;
      font-size: var(--font-size-medium);
      text-align: right;
      padding-right: 10px;
   }


   .converter__input--nok {
      display: none;
      width: 272px;
      height: 68px;
      background: var(--main-color);
      border: none;
      font-family: monospace;
      font-size: var(--font-size-medium);
      text-align: right;
      padding-right: 10px;
   }   

   .converter__equal {
      font-size: var(--font-size-large);
      text-align: center;
      border: solid;
      height: fit-content;
   }

   .converter__output {
      width: 272px;
      height: 68px;
      background: var(--main-color);
      color: var(--faded-text-color);
      border: none;
      font-family: monospace;
      font-size: var(--font-size-medium);
      text-align: right;
      padding-right: 10px;
   }

   .converter__buttons {
      display: flex;
      justify-content: center;
      height: fit-content;
      position: relative;
   }

   .converter__buttons button {
      background: none;
      border: none;
   }

   .converter__hamburger {
      z-index: 1;
   }

   .converter__navigation {
      display: none;
      position: absolute;
      overflow: scroll;
      width: 100vw;
      height: 77vh;
      padding: 20px;
      background: var(--main-color);
      top: 210px;
   }

   .navigation__button {
      font-family: monospace;
      font-size: var(--font-size-small);
      margin-bottom: 10px;
   }
</style>

<script>
	export default {
		data() {
			return {
           inputValue: '',
           outputValue: 'nok',
           multipleCurrency: '',
           togleEur: true,
           togleNok: false,
         }
      },

	async created() {
		this.fetchNew();
      this.hei();
	},

   methods: {
		async fetchNew() {
			
			const url = 'https://cdn.jsdelivr.net/gh/fawazahmed0/currency-api@1/latest/currencies/eur.json';
			const res = await fetch(url);
			const { eur } = await res.json();

         // 

			this.outputValue = (this.inputValue * eur.nok).toFixed(2);

         // henter data og setter verdien til output til kursen ganget med ønsket antall
      },

      async convertNok() {

			const url = 'https://cdn.jsdelivr.net/gh/fawazahmed0/currency-api@1/latest/currencies/eur.json';
			const res = await fetch(url);
			const { eur } = await res.json();

			this.outputValue = (this.inputValue / eur.nok).toFixed(2);

         // henter data og setter verdien til output til kursen delt med ønsket antall
      },
   }
}
</script>
