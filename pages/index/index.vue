<template>
  <div class="exchange-rate">
    <h1>{{ sgdCurrencyCode }} & MYR</h1>
	<br/>
    <div>
      <strong>Date:</strong> {{ sgdRate.date }}<br />
      <strong>Buying Rate:</strong> {{ sgdRate.buying_rate }}<br />
      <strong>Selling Rate:</strong> {{ sgdRate.selling_rate }}<br />
      <strong>Middle Rate:</strong> {{ sgdRate.middle_rate }}<br />
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      sgdCurrencyCode: null,
      sgdRate: null,
    };
  },
methods: {
  async fetchExchangeRates() {
    const url = 'https://api.bnm.gov.my/public/exchange-rate';
    const params = new URLSearchParams({
      session: '0900',
      quote: 'rm',
    });

    try {
      const response = await fetch(`${url}?${params.toString()}`, {
        headers: {
          'Sec-Ch-Ua': '"Not:A-Brand";v="99", "Chromium";v="112"',
          'Accept': 'application/vnd.BNM.API.v1+json',
          'Sec-Ch-Ua-Mobile': '?0',
          'User-Agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/112.0.5615.50 Safari/537.36',
          'Sec-Ch-Ua-Platform': '"Windows"',
          'Origin': 'https://apikijangportal.bnm.gov.my',
          'Sec-Fetch-Site': 'same-site',
          'Sec-Fetch-Mode': 'cors',
          'Sec-Fetch-Dest': 'empty',
          'Referer': 'https://apikijangportal.bnm.gov.my/',
          'Accept-Encoding': 'gzip, deflate',
          'Accept-Language': 'en-US,en;q=0.9',
          'Connection': 'close',
        },
      });

      if (response.ok) {
        const data = await response.json();
        const sgdEntry = data.data.find((currency) => currency.currency_code === 'SGD');
        this.sgdCurrencyCode = sgdEntry.currency_code;
        this.sgdRate = sgdEntry.rate;
      } else {
        console.error(`Error: ${response.status}`);
      }
    } catch (error) {
      console.error('Error:', error);
    }
  },
},
  mounted() {
    this.fetchExchangeRates();
  },
};
</script>

<style scoped>
.exchange-rate {
  text-align: center;
  margin-top: 50px;
  font-family: Arial, sans-serif;
}
</style>
