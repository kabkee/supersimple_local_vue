<template>
    <b-container>
		<b-form autocomplete="on" @submit="false">
        <b-row class="my-1">
            <b-col sm="2">
                <label for="input_search">제목검색:</label>
            </b-col>
            <b-col sm="10">
                <b-form-input id='input_search' v-model="search" placeholder="제목검색"></b-form-input>
            </b-col>
        </b-row>
        <b-row class="my-1">
            <b-col sm="2">
                <label for="input_search_number">숫자검색:</label>
            </b-col>
            <b-col sm="10">
                <b-form-input id='input_search_number' v-model="search_number" placeholder="숫자검색"></b-form-input>
            </b-col>
        </b-row>
		</b-form>
        <b-row>
            <b-col class="allSongs" cols="3" v-for="song in filteredSongs" :key="song.id">
                <router-link :to="`/song/${song.id}`">
                    <b-img
                        class="thumb"
                        v-bind="mainProps"
                        rounded="circle"
                        alt="Circle image"
                        :src="`/data/song_${song.id}/${song.thumb}`"
                        style="max-width: 100%;"
                    ></b-img>
					<h5><b-badge v-if='!song.video' variant='danger'>동영상 없음</b-badge></h5>
                    <span class="title">{{ `No.${song.id+1}` }} {{ song.title }}</span>
                </router-link>
            </b-col>
        </b-row>
		<b-row v-if='!filteredSongs.length'>
            <b-col class="noSong">
				<h5 style='color: grey;'>등록된 노래가 없습니다.</h5>
				<h5 v-if='search_number' style='color: grey;'>검색가능 최대 번호는 1 ~ {{ songs.length }}입니다.</h5>
				
            </b-col>
        </b-row>
    </b-container>
</template>

<script>
import song_meta from '../assets/data_meta.json';

export default {
    name: 'Home',
    data() {
        return {
            publicPath: process.env.BASE_URL,
            songs: [],
            previews: [],
            mainProps: {
                blank: false,
                blankColor: '#777',
                class: 'm1'
			},
			search: null,
			search_number: null,
        };
	},
	computed: {
		filteredSongs(){
			if(this.search || this.search_number){
				return this.songs.filter((song)=>{
					return this.search?song.title.toLowerCase().includes(this.search.toLowerCase()):false || parseInt(this.search_number) === parseInt(song.id) +1
				})
			}
			return this.songs;
		},
	},
    mounted() {
        this.songs = song_meta.filter(song => {
            return song.thumb;
        });
    }
};
</script>

<style>
.allSongs a {
    display: block;
    margin: 15px 30px;
    transition: 0.4s all ease;
    float: left;
    width: 100%;
    text-align: center;
}
.allSongs a:hover .thumb {
    box-shadow: 1px 1px 4px rgba(0, 0, 0, 0.8);
    transform: scale(1.1);
    transition: all 0.1s ease-in-out;
}
.allSongs .thumb {
    transition: all 0.1s ease-in-out;

    display: block;
    border-radius: 50%;
    width: 100%;
    height: auto;
    margin: 0 auto;
    box-shadow: 2px 2px 6px rgba(0, 0, 0, 0.2);
}
.allSongs .title {
    font-size: 14px;
    margin-top: 10px;
    clear: both;
    color: #00609c;
    display: block;
}
.noSong{
	margin-top: 1em;
}
</style>
