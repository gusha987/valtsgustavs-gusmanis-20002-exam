<template>
  <div id="about-view">
      <div class="wrapper-header">
          <h1>ABOUT ME</h1>
          <div class="settings">
              <div v-if="groupy"><button id="btn-show-favorites" @click="editButt()">Edit Form</button></div>
              <div v-if="group"><button id="btn-show-favorites" @click="cancelBut()" :class="{active: group}">Cancel</button></div>
              <div v-if="group"><button id="btn-save" @click="saveBut()">Save Form</button></div>
          </div>
      </div>
      <form>
          <div class="wrapper-input">
              <label>NAME</label>
              <div v-if="group"><input v-model="chName" id="input-name" /></div>
              <div v-if="groupy"><p id="txt-name">{{ auth.user.name }}</p></div>
          </div>
          <div class="wrapper-input">
              <label>SURNAME</label>
              <div v-if="group"><input v-model="chSurName" id="input-surname" /></div>
              <div v-if="groupy"><p id="txt-surname">{{ auth.user.surname }}</p></div>
          </div>
          <div class="wrapper-input">
              <label>STUDENT CODE</label>
              <div v-if="group"><input v-model="chCode" id="input-code" /></div>
              <div v-if="groupy"><p id="txt-code">{{ auth.user.code }}</p></div>
          </div>
          <div class="wrapper-songs">
              <label>FAVORITE SONGS</label>
              <ul v-if="auth.getFavoriteSongs().length">
                  <li v-for="song in favo_songs">
                      <img id="img-album" :src="song.album.images[1].url" />
                      <div class="song-info">
                          <p id="txt-song" class="song-name">{{ song.name }}</p>
                          <p id="txt-artist" class="song-artists">{{ getArtists(song.artists) }}</p>
                      </div>
                  </li>
              </ul>
              <div id="txt-empty" class="empty">NO SONGS FOUND</div>
          </div>
      </form>
  </div>
</template>

<script>
import { auth } from '../stores/auth'
import songList from '../data/songs'
export default {
  data() {
    return {
      auth,
      songs: songList,
      group: false,
      groupy: true,
      chName: auth.user.name,
      chSurName: auth.user.surname,
      chCode: auth.user.code,
      isFavLiskty: true
    }
  },
  methods: {
      editButt() {
        this.group = true;
        this.groupy = false;
      },
      cancelBut() {
        this.group = false;
        this.groupy = true;
      },
      saveBut() {
        this.group = false;
        this.groupy = true;
        this.auth.setUserData(this.chName, this.chSurName, this.chCode);
      },
      getArtists(artists) {
      let k = '';
      let len = Object.keys(artists).length;

      artists.forEach((a, i) => {
        if (i != len - 1) {
          k = k + a.name + ", ";
        } else {
          k = k + a.name;
        }
      });
      return k;
    },
  },
  
  computed: {
    favo_songs() {
      let favo_songs = [];
      let k = auth.getFavoriteSongs();
      
      k.forEach((songID) => {
        this.songs.forEach((song) => {
          if (song.id == songID) {
            favo_songs.push(song);
          }
        });
      });
      return favo_songs;
    }
  }
}
</script>
