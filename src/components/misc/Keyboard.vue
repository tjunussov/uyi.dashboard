<template lang="pug">
.keyboard {{keyText}}
</template>

<script>

var captureTM;

export default {
  name: 'Keyboard',
  data () {
    return {
      keyText:""
    }
  },
  created(){
    window.addEventListener('keydown',this.documentKeydown)
  },
  beforeDestroy(){
    window.removeEventListener('keydown',this.documentKeydown)
  },
  mounted(){
    // console.log('mouted qr',this.value);
    if(this.value) this.encode()
  },
  methods:{
    documentKeydown(event){

      if (event.keyCode == 13) { // ENTER
        //if(!isNaN(this.keyText)) {
        if(this.keyText != "") {
          
          
          this.$bus.$emit('keyboard:keydown:enter:'+this.keyText.length,this.keyText);
          // this.$bus.$emit('keyboard:keydown:enter:'+this.keyText.substr(0,1),this.keyText.substr(1));
          this.$bus.$emit('keyboard:keydown:enter',this.keyText);
          this.keyText = ""
          event.preventDefault();
        }
      } else {

        if( event.keyCode >= 96 && event.keyCode <=105 ){ // only numpad and top numbers
          this.keyText += String.fromCharCode(event.keyCode-48);
        } else if( event.keyCode >= 65 && event.keyCode <=90 ){ // only alphanumerics
          this.keyText += String.fromCharCode(event.keyCode+32);
        } else if( event.keyCode >= 48 && event.keyCode <=57 ) { // numbers
          this.keyText += String.fromCharCode(event.keyCode);
        }

        if(event.keyCode == 8 ){ // ????
          this.keyText = this.keyText.slice(0,-1)
        }
      }

      window.clearTimeout(captureTM)
      captureTM = window.setTimeout(()=>{
        this.keyText = ""
      },1000)
    },
  }
}


</script>

<style lang="stylus">

.keyboard
  font-size 10rem
  line-height 10rem
  // margin-bottom -1rem
  letter-spacing -1rem
  opacity 0.5
  color white
  overflow hidden
  position absolute
  right 0
  bottom 0
  left 0
  z-index 1000
  
</style>
