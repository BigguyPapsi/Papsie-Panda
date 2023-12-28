<template>
  <div class="container mt-5">

 
    <div style="margin-right: 65px; display: flex; justify-content: space-between; margin-bottom: 100px;">
      <div>
      <td><a href="#"><img src="../assets/panda_8372043.png" style="width: 55px;" /></a></td>
      <td style="padding-left: 10px;   pointer-events: none;"> Papsie.Panda</td>
    </div>

    <div>
    <td style="border-right: 2px solid rgb(50, 50, 50);">
  <input v-model="search" type="search" class="custom-input" id="exampleInput" placeholder="Search" @input="filterItems" style="padding-left: 15px; margin-right: 25px;">
    
    </td>

<td>
  <a href="https://github.com/BigguyPapsi" target="_blank"><img src="../assets/github.svg" style="width: 35px; margin-left: 25px;" /></a>
  <a href="https://www.facebook.com/profile.php?id=100068467746235&locale=th_TH" target="_blank"><img src="../assets/facebook.svg" style="width: 35px; margin-left: 25px;" /></a>
</td>
    
  </div>
      
  
  </div>

   <div class="title"><b>FIGMA FILE</b></div>
   <div style="display: flex; justify-content: space-between;">
     <td><p style="font-size: 25px; margin-bottom: 35px;">

      <td style="pointer-events: none; opacity: 55%;"><b style="border-right: 3px solid rgb(50, 50, 50); padding-right: 25px;">File UX-UI ທັ້ງໝົດ</b></td>
      <td style="padding-left: 25px;">
        <a href="https://docs.google.com/spreadsheets/d/1PNqgq_hHX7cH9qZymkn4X_Iq1FjpujTY/edit#gid=300519293" 
        style="text-decoration: none; color: #0D6EFD;" target="_blank">
        My work 
        <i class="bi bi-chevron-right"/>
      </a>
    </td>
  </p>
</td>

  
  <td>
    <div class="pagination" style="margin-right: 65px;">

          <button class="btn btn-outline-secondary" v-if="currentPage === 1" disabled @click="changePage(currentPage - 1)">
            <i class="bi bi-chevron-left"></i>
          </button>
             <a class="btn btn-outline-primary" v-else @click="changePage(currentPage - 1)">
            <i class="bi bi-chevron-left"></i>
          </a>

          <div>
            <span class="page-number btn-primary btn" style="cursor: default; background-color: #1B1A1F; border: 1px solid #0D6EFD;">
              {{ currentPage }}
            </span>
          </div>


          <button class="btn btn-outline-secondary" v-if="currentPage === pageCount" disabled @click="changePage(currentPage + 1)">
            <i class="bi bi-chevron-right"></i>
          </button>

          <a class="btn btn-outline-primary" v-else @click="changePage(currentPage + 1)">
            <i class="bi bi-chevron-right"></i>
          </a>
          
        </div>



  </td>
  </div>


      <!-- <div v-if="isLoading" class="spinner-border" role="status">
        <span class="visually-hidden"></span>
      </div> -->

           

<div style="height: 720px;">

  <div v-if="isLoading" style="display: flex; justify-content: center; padding-top: 120px;">
  <div class="loader" ></div>
</div>
      <div class="row">
    <div v-for="item in displayedItems" :key="item.id" class="col-md-4 mb-3">
      <div class="card mb-2" style="max-width: 540px; background-color: #212227;">
  <div class="row g-0">
    <div class="col-md-4">
      <img :src="item.image" class="img-fluid rounded-start imgSty" alt="image broken">
    </div>
    <div class="col-md-8">
      <div class="card-body">
        <h5 class="card-title a" style="color: aliceblue; pointer-events: none;">{{ item.name }}</h5>
        <p class="card-text b" style="color: rgb(176, 176, 176); pointer-events: none;">{{ item.description }}</p>
        <div style="display: flex; justify-content: end;"><a :href=item.url class="btn btn-primary" target="_blank">
          ເປີດເບິ່ງ
          <i class="bi bi-box-arrow-up-right"></i>
        </a> </div>
      </div>

    </div>
    
  </div>
</div>




    </div>
  </div>
</div>








      </div>


</template>

<script>
import axios from 'axios';
export default {
  data() {
    return {

      items: [],
      isLoading: false,
      search: '',
      perPage: 9, // Set to display 4 rows and 3 columns
      currentPage: 1,
    };
  },
  mounted() {
    this.fetchPosts();
  },
  computed: {
    filteredItems() {
      const searchLower = this.search.toLowerCase();
      return this.items.filter(item => item.name.toLowerCase().includes(searchLower));
    },
    pageCount() {
      return Math.ceil(this.filteredItems.length / this.perPage);
    },
    displayedItems() {
      const startIndex = (this.currentPage - 1) * this.perPage;
      const endIndex = startIndex + this.perPage;
      return this.filteredItems.slice(startIndex, endIndex);
    },
  },
  methods: {
    filterItems() {
      this.currentPage = 1;
    },
    changePage(page) {
      if (page >= 1 && page <= this.pageCount) {
        this.currentPage = page;
      }
    },


    fetchPosts() {
      // Set loading state to true
      this.isLoading = true;
      axios.get('https://script.googleusercontent.com/macros/echo?user_content_key=-enCkswhdE7TJ2oixvtaaEJ6BxN_6MMQyqcmX7oxqIyhpffYyc9nin0b7gg7X2Y-awkJSMPA1RPew2xLAaG-xi3QPQDO4KLLm5_BxDlH2jW0nuo2oDemN9CCS2h10ox_1xSncGQajx_ryfhECjZEnF1LVLxYlVNUj9MrlRHnIR9mcbuhK6_UqpltWzU8jedfyTh8_P70cHGdBsrydCQNTPZJY0nPHCNu1-8e40Ay1R9IBxhFJkIJ8Q&lib=MiA89kT0xelWqwyyPxvAbLC0dx0bejefQ')
        .then(response => {
          // Handle the successful response
          this.items = response.data.reverse();
        })
        .catch(error => {
          // Handle errors
          console.error('Error fetching posts:', error);
        })
        .finally(() => {
          // Set loading state to false after the request completes
          this.isLoading = false;
        });
    },




  },
};
</script>

<style>



.title {
  background: #1626FF;
background: linear-gradient(to right, #1626FF 0%, #F728AB 50%);
-webkit-background-clip: text;
-webkit-text-fill-color: transparent;
font-size: 100px;
pointer-events: none;

}



.a {
  white-space: nowrap; 
  width: 200px; 
  overflow: hidden;
  text-overflow: ellipsis; 
}




.b {
      width: 200px;
      display: -webkit-box;
      -webkit-box-orient: vertical;
      height: 100px;
      overflow: hidden;
      -webkit-line-clamp: 4; /* Number of lines to show */
    }


/* Add your custom styles here */
.pagination {
  margin-top: 10px;
  display: flex;
  justify-content: center;
  align-items: center;
}

.page-number {
  margin: 0 10px;
}


.imgSty {
  height: 219px;
  width: 120px;
  object-fit: cover;
}


.custom-input {
      transition: 0.3s;
      background-color: #161618;
      color: #ffffff; 
      width: 200px;
      transition: width 0.3s ease;
      border: 2px solid #161618;
      height: 50px;
      border-radius: 5px;
    }
    .custom-input:focus {
      background-color: #161618; 
      border: 2px solid #161618;
      color: #ffffff; 
      width: 300px;


    }

    ::placeholder {
      padding-left: 10px;
      color: rgb(188, 188, 188);
      opacity: 1; /* Firefox */
    }

    .card{
      width: 350px;
      height: 220px;
    }





/* spiner---------------------------------------------------------------------------------------> */

.loader {
  border: 16px solid #f3f3f3;
  border-radius: 50%;
  border-top: 16px solid #4c40cf;
  width: 120px;
  height: 120px;
  -webkit-animation: spin 2s linear infinite; /* Safari */
  animation: spin 2s linear infinite;
}

/* Safari */
@-webkit-keyframes spin {
  0% { -webkit-transform: rotate(0deg); }
  100% { -webkit-transform: rotate(360deg); }
}

@keyframes spin {
  0% { transform: rotate(0deg); }
  100% { transform: rotate(360deg); }
}
      

</style>
