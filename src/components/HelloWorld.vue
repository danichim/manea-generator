<template>
  <div class="hello container">
    <h1>{{ msg }}</h1>

    <div class="input-group mb-3">
      <div class="input-group-prepend">
        <label class="input-group-text" for="inputGroupSelect01">Optiuni</label>
      </div>
      <select
        class="custom-select"
        id="inputGroupSelect01"
        v-model="selectedOption"
      >
        <option value="1">tipuri de manele</option>
        <option value="2">tipuri de artisti</option>
      </select>
    </div>

    <div v-if="selectedOption === '1'" class="tipuridemanele">
      <p>Available types are: 'Vechi', 'Noi', 'Populare'</p>
      <div>
        <label for="genurimuzica">ia manele: </label>
        <input
          id="genurimuzica"
          placeholder="Tasteaza..."
          v-model="txtTypes"
          size="medium"
        />
        <input
          id="catepiese"
          placeholder="1"
          v-model="cateManele"
          size="medium"
          type="number"
          min="1"
          max="10"
          @change="getUrlTypes(txtTypes, cateManele);"
        />
        <button
          @click="getUrlTypes(txtTypes, cateManele);"
          type="button"
          class="btn btn-sm btn-outline-secondary"
        >
          Altele!
        </button>
      </div>
      <div v-text="helper" v-if="txtTypes.length === 0"></div>
      <div v-text="infoTypes" v-if="typeof this.infoTypes === 'string'"></div>
      <hr />
      <div
        v-if="typeof this.info != 'string'"
        v-for="(info, index) in infoTypes"
        :key="info.id"
        class="listing"
      >
        <a :href="info.url" target="_blank">{{ index + 1 }}. {{ info.name }}</a>
      </div>
    </div>

    <div v-if="selectedOption === '2'" class="iamaneadela">
      <p>
        Available manelists are: 'Florin Salam', 'Nicolae Guta', 'Sorinel
        Pustiu'
      </p>
      <div>
        <label for="iamaneadela">ia manea de la: </label>
        <input
          id="iamaneadela"
          placeholder="Tasteaza..."
          v-model="txt"
          size="medium"
        />
        <button
          @click="getUrlDela(txt);"
          type="button"
          class="btn btn-sm btn-outline-secondary"
        >
          alta!
        </button>
      </div>
      <div v-text="helper" v-if="txt.length === 0"></div>
      <div v-text="info" v-if="typeof this.info === 'string'"></div>
      <hr />
      <div v-if="typeof this.info != 'string'">
        <a :href="info.url" target="_blank">{{ info.name }}</a>
      </div>
    </div>

    <hr />

    <FooterInfo />
  </div>
</template>

<script>
import manea from "manea";
import FooterInfo from ".././components/FooterInfo";

export default {
  name: "HelloWorld",
  data() {
    return {
      msg: "Bun venit sefule pe Youtube manea G3n3r4t0r",
      txt: "Nicolae Guta",
      txtTypes: "Vechi",
      info: "",
      helper: "",
      selectedOption: "1",
      cateManele: 2,
      infoTypes: ""
    };
  },
  methods: {
    getUrlDela(txt) {
      this.info = manea.iaManeauaDeLa(txt);
    },
    getUrlTypes(txtTypes, cateManele) {
      this.infoTypes = manea.iaManele(txtTypes, cateManele);
    }
  },
  watch: {
    txt: function() {
      if (this.txt.length === 0) {
        this.helper = "Ce cauti sefule te rog sa scrii!";
        return;
      }
      this.helper = "";
      this.getUrlDela(this.txt);
    },
    txtTypes: function() {
      if (this.txtTypes.length === 0) {
        this.helper = "Ce cauti sefule te rog sa scrii!";
        return;
      }
      this.helper = "";
      this.getUrlTypes(this.txtTypes, this.cateManele);
    }
  },
  components: {
    FooterInfo
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1,
h2 {
  font-weight: normal;
}
</style>
