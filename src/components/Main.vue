<template>
  <div>
    <Header/>
    <div class="pad-15-hor pad-15-ver search-parent">
      <div class="search-bar">
        <b-form-input
          @input="search_text()"
          v-model="search.text"
          type="text"
          placeholder="Search by Name"
        ></b-form-input>
        <span class="search-icon">
          <i class="fas fa-search"></i>
        </span>
      </div>
      <div>
        <span class="bold">Registros:</span>
        {{comentario.count}}
      </div>
      <div>
        <b-form-select @input="sort()" v-model="search.filter" :options="options"/>
      </div>
    </div>

    <div class="container-fluid">
      <div class="row">
        <div class="col-md-6 pad-15-ver" v-for="wonder in wonders_data" :key="wonder.id">
          <div
            class="card-inner"
            @mouseover="show_hover(true,wonder.comentario)"
            @mouseout="show_hover(false,0)"
          >
            <img class="card-img" :src="wonder.imageURL">

            <div class="card-bottom pad-15-hor" v-show="!hover_flag || active_id != wonder.id">

              <div>
                <span class="bold"><small>Usuario:</small> {{wonder.usuario}}</span>
                <br />
                <span class="bold"><small>Comentario:</small> {{wonder.comentario}}</span>
                <br />
                <span class="bold"><small>Fecha:</small> {{wonder.fecha}}</span>
              </div>
            </div>


          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import Header from "@/components/Header.vue";

import axios from "axios";

export default {
  name: "Main",
  mounted() {
    this.hover_flag = false;

    var inside = this;

    axios
      .get("https://localhost:44374/api/values/GetData?pattern=%")
      .then(function(response) {
        //console.log(response.data.result);

        inside.wonders_data_actual = response.data.result;
        //response.data.data.map(function(wonder) {
        //  inside.comnetario;
        //});

       //inside.wonders_data_actual = inside.wonders_data_actual.map(function(wonder) {
       //  //wonder.active_like = false;
       //  return wonder;
       //});
        inside.wonders_data = response.data.result;
      })
      .catch(function(error) {
        alert(error);
      });
  },
  data() {
    return {
      hover_flag: false,
      wonders_data_actual: [],
      wonders_data: [],
      active_id: 0,
      options: [
        { value: null, text: "Sort By" },
        { value: "a", text: "fecha" },
        { value: "b", text: "Likes" }
      ],
      search: { filter: null, text: "" },
      comentario: { count: 0, hit: 0 }
    };
  },
  methods: {
    show_hover(flag, active_id) {
      this.hover_flag = flag;
      this.active_id = active_id;
    },
    sort() {
      //console.log(this.search.filter);
      (this.search.filter == "a")
        ? this.wonders_data.sort( (a, b) => {
            return new Date(a.fecha) < new Date(b.fecha);
        })
        
        : '';
        
        return this.wonders_data;
    },
    search_text() {
      //console.log(this.search.text);

      var inside = this;

      this.wonders_data = this.wonders_data_actual.filter(function(wonder) {
        if (
          wonder.comentario
            .toLowerCase()
            .indexOf(inside.search.text.toLowerCase()) != "-1"
        ) {
          return wonder;
        }

        if (
          wonder.usuario
            .toLowerCase()
            .indexOf(inside.search.text.toLowerCase()) != "-1"
        ) {
          return wonder;
        }

        if (
          wonder.fecha
            .toLowerCase()
            .indexOf(inside.search.text.toLowerCase()) != "-1"
        ) {
          return wonder;
        }

      });
    }
    ,
    //check_active(id) {
    //  var flag = false;
    //  this.wonders_data_actual.map(function(wonder) {
    //    if (wonder.id == id) {
    //      flag = wonder.active_like;
    //    }
    //  });
    //  return flag;
    //},
  //  make_active(id) {
  //    this.likes.hit++;
  //    this.wonders_data_actual = this.wonders_data_actual.map(function(wonder) {
  //      if (wonder.id == id) {
  //        wonder.active_like = !wonder.active_like;
  //        wonder.active_like ? wonder.likes++ : wonder.likes--;
  //      }
//
  //      return wonder;
  //    });
  //    var inside = this;
//
  //    inside.likes.count = 0;
  //    this.wonders_data_actual.map(function(wonder) {
  //      inside.likes.count += wonder.likes;
  //    });
  //  }
  },
  components: {
    Header
  }
};
</script>




<style scoped>
.header {
  display: flex;
  flex-flow: row wrap;
  justify-content: space-between;
}

.search-parent {
  display: flex;
  flex-flow: row wrap;
  justify-content: space-between;
  background-color: lightgray;
}

.card-inner {
  position: relative;
  overflow: hidden;
  box-shadow: 2px 2px 8px grey;
}

.card-img {
  width: 100%;
}

.card-bottom {
  bottom: 0;
  left: 0;
  width: 100%;
  background-color: white;
  opacity: 0.7;
  display: flex;
  justify-content: space-between;
  overflow: hidden;
}

.card-hover {
  position: absolute;
  right: 15px;
  left: 15px;
  top: 15px;
  bottom: 15px;
  background-color: white;
  opacity: 0.7;
  display: flex;
  flex-flow: column wrap;
  justify-content: center;
  align-items: center;
}
.absolute-star {
  position: absolute;
  top: 10px;
  right: 10px;
}

.card-hover p {
  font-size: 10px;
  text-align: center;
}

.bold {
  font-weight: 500;
}

.rating-div {
  width: 200px;
}

.search-bar {
  position: relative;
}
.search-bar input {
  padding-left: 30px;
}

.search-icon {
  position: absolute;
  top: 8px;
  left: 8px;
}

@media screen and (max-width: 550px) {
  .search-parent {
    display: flex;
    flex-flow: column wrap;
    justify-content: center;
    align-items: center;
    background-color: lightgray;
  }

  .search-parent div {
    width: 100%;
    text-align: center;
  }
}
</style>


