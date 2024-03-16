<template>

    <body>
        <div class="container">
            <div class="row">

                <div>
                    <input id="url" type="text" placeholder=" Coloque a Url do Vídeo aqui">
                </div>
                <div>
                    <button class="btn" v-show="show_buscar" @click="Pegar_video">Buscar</button>


                    <button class="btn" v-show="show_Download" @click="baixarVideo">Download</button>
                </div>
                <p id="resultado"></p>
            </div>

            

            <div id="videoContainer">

            </div>



        </div>
    </body>


</template>

<script>
import axios from 'axios';

export default {
    name: "corpo",

    data() {
        return {
            show_Download: false,
            show_buscar: true,
        }
    },

    methods: {
        Pegar_video() {
            //Pegando o ID da Url do vídeo
            var Url = document.getElementById('url').value;
            // eslint-disable-next-line no-useless-escape
            var regExp = /^(?:https?:\/\/)?(?:www\.)?(?:youtube\.com\/(?:[^\/\n\s]+\/\S+\/|(?:v|e(?:mbed)?)\/|\S*?[?&]v=)|youtu\.be\/)([a-zA-Z0-9_-]{11})/;
            var match = Url.match(regExp);
            if (match) {
                var videoID = match[1];
                document.getElementById('resultado').innerText = "Video resgatado com sucesso";
                document.getElementById('resultado').style = "color:green; font-weight: 800";
                this.show_Download = true;
                this.show_buscar = false;

                setTimeout(function () {
                    document.getElementById('resultado').innerText = "";
                }, 3000);




            }
            else {
                document.getElementById('resultado').innerText = "Link errado ou sem suporte";
                document.getElementById('resultado').style = "color:red; font-weight: 800";
                setTimeout(function () {
                    document.getElementById('resultado').innerText = "";
                }, 3000);
                this.show_Download = false;
                this.show_buscar = true;
            }

            //Criar o iframe com o vídeo do youtube baseado no videoID
            var iframe = document.createElement('iframe');
            iframe.style = "border-radius: 15px; border-style: none; box-shadow: 1px 1px 10px black; width: 50%; height: 480px"
            iframe.src = 'https://www.youtube.com/embed/' + videoID;
            iframe.allowfullscreen = false;
            document.getElementById('videoContainer').innerHTML = '';
            document.getElementById('videoContainer').appendChild(iframe);
        },
        baixarVideo() {
            var Url = document.getElementById('url').value;
            // eslint-disable-next-line no-useless-escape
            var regExp = /^(?:https?:\/\/)?(?:www\.)?(?:youtube\.com\/(?:[^\/\n\s]+\/\S+\/|(?:v|e(?:mbed)?)\/|\S*?[?&]v=)|youtu\.be\/)([a-zA-Z0-9_-]{11})/;
            var match = Url.match(regExp);
            if (match) {
                var videoID = match[1];
                //Esse get da api/youtube/baixarvideo vai baixar o video no backend e voltar o video para o front
                axios.get(`http://localhost:5070/api/youtube/baixarvideo?videoID=${videoID}&Url=${Url}`, { responseType: 'blob' })
                    .then(response => {
                        //Aqui eu busco o titulo do video
                        this.buscarTitulo(Url)
                            .then(result => {
                                //Aqui eu monto os bytes em video e envio para o usuario
                                const url = window.URL.createObjectURL(new Blob([response.data]));
                                const link = document.createElement('a');
                                link.href = url;
                                link.setAttribute('download', result + ".mp4");
                                document.body.appendChild(link);
                                link.click();
                                link.remove();
                                this.removerVideo();
                            })
                            .catch(error => {
                                console.log(error);
                            });
                    })
                    .catch(error => console.error(error));
            }
        },

        async buscarTitulo(Url) {
            try {
                const response = await axios.get(`http://localhost:5070/api/youtube/obtertitulo?Url=${Url}`);
                const dados = response.data;
                return dados;
            } catch (error) {
                console.error('Erro:', error);
            }
        },
        removerVideo(){
            //Aqui eu solicito que rode o metodo para remover o video na pasta do servidor, assim evitando deixar full o armazenamento
            axios.post(`http://localhost:5070/api/youtube/removervideo`)
        }

    }

}


</script>


<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Press+Start+2P&display=swap');

body {
    height: 100vh;
    overflow: hidden;

}

.container {
    text-align: center;
    width: 100%;
    height: 100%;


}

.container div {
    padding: 15px;
}

.container a {
    font-size: 20px;
}

#url {
    width: 40%;
    height: 50px;
    border-style: none;
    /* box-shadow: 0.01px 0.01px 20px black; */
    font-size: 15px;
    background-color: #2a2333;
    /* 2a2333 */
    border-radius: 25px;
    font-family: "Press Start 2P", system-ui;
    transition: 0.3s;
}

#url:focus {
    box-shadow: 1px 0px 20px black;
    border: 0 none;
    outline: 0;
}

.btn {
    height: 30px;
    width: 100px;
    font-size: 11px;
    font-family: "Press Start 2P", system-ui;
    border-radius: 10px;
    background-color: white;
    border-style: none;
    transition: 0.3s;
}

.btn:hover {
    background-color: rgb(197, 197, 197);
}


.videoContainer {
    width: 70%;
    height: 20%;
    border-radius: 15px;
    box-shadow: 1px 1px 10px black;
}
</style>