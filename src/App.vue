<template>
  <div>
    <Navbar /> 

    <Event :text="text" />

    <SearchBar :movies="movies_temp" 
    @searchMovie="searchMovie($event)"/> <!--영화 데이터 넘겨주기-->

    <p>
      <button @click="showAllMovie">전체보기</button>
    </p>

    <Movies 
    :movies="movies_temp" 
    @openModal="isModal=true; selectedMovie=$event"
    @incrementLike="incrementLike($event)" />

    <!-- 자식에게 변수 값 전달하기 -->
    <Modal 
    :movies="movies" 
    :isModal="isModal" 
    :selectedMovie="selectedMovie"
    @closeModal="isModal=false" />
   
  </div>
</template>

<script>
import movies from './assets/movies';
import Navbar from './components/Navbar.vue';
import Modal from './components/Modal.vue';
import Movies from './components/Movies.vue';
import Event from './components/Event.vue';
import SearchBar from './components/SearchBar.vue';

export default {
  name: 'App',
  data() {
    return {
      isModal: false,
      selectedMovie: null, 
      movies: movies, // 원본 데이터
      movies_temp: [...movies], // 사본 데이터
      text: "Neplix 강렬한 운명의 드라마, 경기크리처"
    }
  },
  methods: {
    incrementLike(movieId) {
      // movieId로 영화 찾기
      const movie = this.movies.find(movie => movie.id === movieId);
      if (movie) {
        movie.like += 1;
      }
    },
    selectMovie(i) {
      this.selectedMovie = i;
      this.isModal = true;
    },
    searchMovie(title) {
      // 영화 제목이 포함된 데이터
      this.movies_temp = this.movies.filter(movie => {
        return movie.title.includes(title);
      })
    },
    showAllMovie() {
      this.movies_temp = [...this.movies]
    }
  },
  components: {
    Navbar: Navbar,
    Modal: Modal,
    Movies: Movies,
    Event: Event,
    SearchBar: SearchBar
  }  
}
</script>

<style>
* {
  box-sizing: border-box;
  margin: 0;
}

body {
  max-width: 768px;
  margin: 0 auto;
  padding: 20px;
}

h1, h2, h3 {
  margin-bottom: 1rem;
}

p {
  margin-bottom: 0.5rem;
}

button {
  margin-right: 10px;
  margin-top: 10px;
}

.item {
  width: 100%;
  border: 1px solid #ccc;
  display: flex;
  margin-bottom: 20px;
  padding: 1rem;
}

.item figure {
  width: 30%;
  margin-right: 1rem;
}

.item img {
  width: 100%;
  height: 260px; /* 고정된 높이 */
  object-fit: cover; /* 이미지가 박스에 맞게 조정됨 */
}

.item .info {
  width: 70%;
}

.bg-yellow {
  padding: 10px 0;
  font-size: 1.5rem; /* 글자 크기를 키움 */
}

.modal {
  background: rgba(0, 0, 0, 0.3);
  position: fixed;
  left: 0;
  top: 0;
  width: 100%;
  height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
}

.modal .inner {
  background: #fff;
  max-width: 500px; /* 모달 창의 최대 너비 */
  width: 90%; /* 기본 너비 */
  padding: 20px;
  border-radius: 10px;
  position: relative; /* 버튼을 상대적으로 위치시키기 위한 설정 */
}

.modal .inner .close-btn {
  position: absolute;
  top: 10px;
  right: 10px;
  background: white;
  border: none;
  font-size: 20px;
  cursor: pointer;
}
</style>