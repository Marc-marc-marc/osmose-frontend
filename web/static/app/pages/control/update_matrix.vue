<template>
  <div>
    <vue-topprogress ref="topProgress"></vue-topprogress>
    <table
      v-if="render"
      class="table table-striped table-bordered table-hover table-sm"
    >
      <thead>
        <tr>
          <th colspan="4" rowspan="4" />
          <th v-for="k in keys" :key="k" class="country">
            <div class="rotate-90">
              <router-link :to="`../errors/?country=${k}&item=xxxx`">
                {{ k }}
              </router-link>
            </div>
          </th>
        </tr>
        <tr v-for="i in 3" :key="i">
          <th
            is="delay"
            v-for="k in keys"
            :key="k"
            :v="stats_country[k][i - 1]"
          >
            {{ stats_country[k][i - 1] | numFormat("0.0") }}
          </th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="r in matrix_keys" :key="r">
          <th style="text-align: left">{{ r }}</th>
          <th is="delay" v-for="i in 3" :key="i" :v="stats_analyser[r][i - 1]">
            {{ stats_analyser[r][i - 1] | numFormat("0.0") }}
          </th>
          <template v-for="k in keys">
            <td is="delay" v-if="matrix[r][k]" :key="k" :v="matrix[r][k][0]">
              <router-link :to="`update/${matrix[r][k][1]}`">
                {{ matrix[r][k][0] | numFormat("0.0") }}
              </router-link>
            </td>
            <td v-else :key="k" />
          </template>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
import Vue from "vue";

import Delay from "../../components/delay.vue";

export default Vue.extend({
  data() {
    return {
      render: false,
    };
  },
  components: {
    Delay,
  },
  mounted() {
    this.$refs.topProgress.start();
    fetch(
      API_URL + window.location.pathname + ".json" + window.location.search,
      {
        headers: new Headers({
          "Accept-Language": this.$route.params.lang,
        }),
      }
    )
      .then((response) => response.json())
      .then((response) => {
        this.$refs.topProgress.done();

        Object.assign(this, response);
        this.render = true;
      });
    document.title = "Osmose - " + this.$t("Last updates");
  },
});
</script>
