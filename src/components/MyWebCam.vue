<template>
  <div>
      <div class="row">
         <div class="col-md-2"></div>
         <div class="col-md-2">
              <h4>WebCam</h4>  
         </div>
         <div class="col-md-2"></div>
         <div class="col-md-2">
            <h4>Photos taken</h4>    
         </div>
      </div>
      <div class="row">  
          <div class="col-md-6">
                <video autoplay id="videoElement"></video>
                <div class="row">
                    <div class="col-md-4">
                        <p>New photo in {{countTimer}}</p>         
                    </div>
                    <div class="col-md-1">
                        <button class="btn btn-primary btn-sm" @click="start">Start</button>           
                    </div>
                    <div class="col-md-1">
                        <button class="btn btn-danger btn-sm" @click="ClearInterval">Stop</button>           
                    </div>
                    <div class="col-md-1">
                        <button class="btn btn-info btn-sm" @click="Clear">Clear</button>           
                    </div>
                </div>
          </div>
        
          <div class="col-md-2">
              <div v-for="(item, index) in images" :key="index">
                <img :src="item.img" class="photo">
                 <b-button block variant="primary" size="sm" @click="saveBlobAsFile('photo', index)">Download</b-button>
                
            </div> 
          </div>
           
        </div>
  </div>
</template>

<script>

export default {
    data(){
        return{
            images: [],
            countTimer: 3,
            maxPhoto: 4,
            countPhoto: 0,
            interval: null,
            arrayCanvas: []
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
         saveBlobAsFile(fileName, index) {
            var link = document.createElement("a");
            document.body.appendChild(link);

            link.setAttribute("href", this.arrayCanvas[index].toDataURL("image/png")
            .replace(/^data:image\/[^;]/, 'data:application/octet-stream'));

            link.setAttribute("download", fileName + '_' + (index+1) +'.png');
            link.click();       
        },
         capture() {
            if(this.countPhoto === this.maxPhoto)
            {
                this.countTimer = 3;
                this.ClearInterval();
            }
            this.countPhoto++
             if(this.countPhoto <= this.maxPhoto)
             {
               this.images.push({
                    img: this.getCanvas().toDataURL(this.screenshotFormat)
                });  
             }  
        },
        Clear(){
            this.images = [];
            this.countTimer = 3;
            this.maxPhoto = 4;
            this.countPhoto = 0;
            this.interval = null;
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
  width: 150px;
}
</style>