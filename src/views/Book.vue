<template>
    <div class="about">
        <v-container>
            <v-subheader class="title">{{ book.title }}</v-subheader>
            <v-img v-if="book.cover" :src="getImage('/books/' + book.cover)" height="200px"></v-img>
            <v-subheader>Information</v-subheader>
            <table class="v-data-table">
                <tbody>
                    <tr>
                        <th class="text-xs-left">Author: </th><td>{{ book.author }}</td>
                    </tr>
                    <tr>
                        <th class="text-xs-left">Publisher: </th><td>{{ book.publisher }}</td> 
                    </tr>
                    <tr>
                        <th class="text-xs-left">Price: </th><td v-if="book.price">Rp. {{ 
                            book.price.toLocaleString('id-ID') }}</td>
                    </tr>
                    <tr>
                        <th class="text-xs-left">Weight: </th><td>{{ book.weight }} kg</td>
                    </tr>
                    <tr>
                        <th class="text-xs-left">Stock: </th><td>{{ book.stock }}</td>
                    </tr>
                    <tr>
                        <th class="text-xs-left">Categories: </th>
                        <td>
                            <span v-for="category in book.categories" :key="category.id">
                                {{ category.name }}
                            </span>
                        </td>
                    </tr>
                </tbody>
            </table>
            <v-subheader>Descripton</v-subheader>
            <p class="body-2"> {{ book.description }}</p>
            <div style="position:relative">
                <v-btn block color="succes" @click="buy" :disabled="book.stock==0">
                <v-icon>save_alt</v-icon> &nbsp;BUY
                </v-btn>
            </div>
    </v-container>
  </div>
</template>

<script>
import { mapActions } from 'vuex';

export default {
  name: 'BookPage',
  data() {
    return {
      book: {},
    };
  },
  methods: {

    getImage(image) {
                if (image != null && image.length > 0) {
                    // Menggunakan base URL dari variabel VUE_APP_BACKEND_URL
                    return "http://bookstore-api.test/image"+ image
                }

                return "/img/unavailable.jpg";
                },

    ...mapActions({
        addCart:'cart/add',
        setAlert:'alert/set',
    }),
    buy() {
      this.addCart(this.book)
      this.setAlert({
        status: true,
        text: 'Added to cart',
        type: 'primary',
      })
        // alert('Buy')
    }
  },
  created() {
    let slug = this.$route.params.slug;
    this.axios
      .get('/books/slug/' + slug)
      .then((response) => {
        let book = response.data.data;
        this.book = book;
      })
      .catch((error) => {
        if (error.response) {
          console.log(error.response);
        } else {
          console.log(error);
        }
      });
  },
};
</script>