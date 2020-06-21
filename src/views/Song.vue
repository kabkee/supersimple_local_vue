<template>
    <b-container>
        <b-row>
            <b-col>
                <h3>
                    <b-button v-if='prevSongId != -1' :href='`/song/${prevSongId}`' variant="outline-secondary" style='margin-right: 0.5em;'> << 이전</b-button>
                    <b-button v-if='nextSongId != -1' :href='`/song/${nextSongId}`' variant="outline-secondary" style='margin-left: 0.5em;'> >> 다음</b-button>
                </h3>
            </b-col>
        </b-row>
        <b-row>
            <b-col sm="8">
                <h2>{{ songData.title }}</h2>
                <video v-if='songData.video' id="myVideo" :src="`/data/song_${songData.id}/${songData.video}`" controls autoplay></video>
                <h3 v-else style='margin-top: 2em; color:#999;'>
                    동영상이 없습니다.
                </h3>
            </b-col>
            <b-col sm="4" style='text-align: left; background-color: #fff; padding: 0.5em;'>
                <h3><b-badge>SONG LYRICS</b-badge></h3>
                <span v-html='songData.lyric_html'></span>
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
            songData: song_meta[this.$route.params.songId],
            songId: parseInt(this.$route.params.songId),
        };
	},
	computed: {
        prevSongId(){
            return this.songId -1
        },
        nextSongId(){
            
            return this.songId + 1 >= song_meta.length ? -1 : parseInt(this.songId) + 1
        }
	},
    mounted() {
    }
};
</script>

<style>
body{
    background-color: #FAFAFA;
}
</style>
