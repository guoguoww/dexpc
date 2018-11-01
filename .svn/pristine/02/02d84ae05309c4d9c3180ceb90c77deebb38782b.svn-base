import Vue from 'vue';
import Vuex from 'vuex';
import axios from "../axios";

Vue.use(Vuex);
export default new Vuex.Store({
  strict: process.env.NODE_ENV !== 'production',
  state: {
    currency: [],
  },
  mutations: {
    getCur(state,params) {
      state.currency = params;
    },
  },
  modules: {

  },
  getters: {
    currency(state) {
      return state.currency;
    }
  },
  actions: {
    setMutation (context) {
      axios.post('/api/trade/currency_list').then(res => {

        context.commit('getCur', {params: res.data.data})

      })
        .catch((err) => {
          console.log(err)
        })
    }
  }
});
