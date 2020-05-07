<template>
  <div>
      <div class="row">  
          <div class="col-md-6">
            <video autoplay id="videoElement"></video>
            <nav aria-label="Page navigation example">
                <ul class="pagination justify-content-center">
                    <li class="page-item disabled">
                        <a class="page-link" href="#" tabindex="-1" aria-disabled="true">photo in {{countTimer}}</a>
                    </li>
                    <li class="page-item">
                        <a class="page-link" href="#" @click="start">Start</a>
                    </li>
                    <li class="page-item">
                        <a class="page-link" href="#" @click="ClearInterval">Stop</a>
                    </li>
                    <li class="page-item">
                        <a class="page-link" href="#" @click="Clear">Clear</a>
                    </li>
                    <li class="page-item" v-if="saveAllPhotos">
                        <a class="page-link" href="#" @click="Save">Save</a>
                    </li>
                </ul>
                </nav>
          </div>
       
          <div class="col-md-2" id="allimages">
            <div v-for="(item, index) in images" :key="index">
                <img :src="item.img" class="photo">
            </div> 
          </div>

           <div class="col-md-2" id="print"></div>
           
        </div>
  </div>
</template>

<script>
import htmlToImage from 'html2canvas';
export default {
    data(){
        return{
            images: [],
            countTimer: 3,
            maxPhoto: 4,
            countPhoto: 0,
            interval: null,
            arrayCanvas: [],
            saveAllPhotos: false
        }
    },
    methods:{
       start(){
          this.interval = setInterval(() => {
                if(this.countTimer === 0){
                    this.countTimer = 3;
                    this.capture();
                }
                this.countTimer--;
            }, 1000)
        },
         capture() {
            if(this.countPhoto === this.maxPhoto)
            {
                this.countTimer = 3;
                this.ClearInterval();
                this.saveAllPhotos = true;
            }
            this.countPhoto++
             if(this.countPhoto <= this.maxPhoto)
             {
               this.images.push({
                    img: this.getCanvas().toDataURL(this.screenshotFormat)
                });  
             }  
        },
        Save(){
           htmlToImage(document.querySelector('#allimages'))
           .then((canvas) => {
                var link = document.createElement("a");
                document.body.appendChild(link);
                link.setAttribute("href", canvas.toDataURL("image/png")
                .replace(/^data:image\/[^;]/, 'data:application/octet-stream'));
                link.setAttribute("download", 'allimages.png');
                link.click();
            });
        },
        Clear(){
            this.images = [];
            this.countTimer = 3;
            this.maxPhoto = 4;
            this.countPhoto = 0;
            this.interval = null;
            this.saveAllPhotos = false;
        },
        ClearInterval(){
             clearInterval(this.interval);
        },
        getCanvas() {
            var video = document.querySelector('#videoElement');
            if (!this.ctx) 
            {
                let canvas = document.createElement("canvas");
                canvas.height = video.videoHeight;
                canvas.width = video.videoWidth;
                this.canvas = canvas;
                this.ctx = canvas.getContext("2d");
            }

            const { ctx, canvas } = this;
            ctx.drawImage(video, 0, 0, canvas.width, canvas.height);
            this.arrayCanvas.push(canvas);
            return canvas;
        },

        loadCam(){
            navigator.getUserMedia = navigator.getUserMedia || navigator.webkitGetUsermedia 
            || navigator.mozGetUserMedia || navigator.msGetUserMedia || navigator.oGetUserMedia;
       
            if(navigator.getUserMedia)
            {
                navigator.mediaDevices.getUserMedia({video:true})
                .then(stream => {
                    var video = document.querySelector('#videoElement');
                    video.srcObject = stream;
                    video.onloadedmetadata = () => {
                        video.play();
                    };
                })    
            }

       }
    },
    created(){
        this.loadCam();
    }
}
</script>

<style>
#container {
	margin: 0px auto;
	width: 500px;
	height: 375px;
	border: 10px #333 solid;
}
#videoElement {
	width: 500px;
	height: 375px;
	background-color: #666;
}
.image {
	width: 500px;
	height: 375px;
	background-color: #666;
}
.photo {
  border: 1px solid #ddd;
  border-radius: 4px;
  padding: 5px;
  width: 100%;
}
</style>