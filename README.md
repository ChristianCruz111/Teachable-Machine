# Pedra, Papel e Tesoura (IA)

Este projeto utiliza inteligência artificial para reconhecer os gestos de **Pedra, Papel e Tesoura** através da câmera. O modelo foi criado e treinado utilizando o **Google Teachable Machine**.

## Classes do Modelo

O modelo foi treinado para reconhecer três gestos:

- Pedra
- Papel
- Tesoura

## Origem dos Dados

As imagens utilizadas no treinamento foram gravadas por mim através da câmera, sem utilizar nenhum dataset externo. Foram utilizados exemplos dos três gestos para treinar o modelo.

## Treinamento

O modelo foi treinado utilizando o Google Teachable Machine. Depois do treinamento, foram realizados alguns testes para verificar se ele conseguia identificar corretamente cada gesto.

## Link do Modelo

https://teachablemachine.withgoogle.com/models/G7JwVrxNr/

## Aplicação

A aplicação utiliza o modelo treinado para identificar, através da câmera, se o gesto apresentado é Pedra, Papel ou Tesoura.


## Reflexão 
Durante os testes, o modelo conseguiu reconhecer a maioria dos gestos corretamente. Porém, ele apresentou dificuldade para identificar o gesto de Papel em alguns momentos.
Acredito que isso aconteceu porque foram utilizadas apenas 55 fotos, o que pode ter sido pouco para o modelo aprender diferentes posições e condições.
Com mais imagens e maior variedade nos exemplos, provavelmente o resultado seria melhor.


 
### Código da aplicação

```html
<div>Teachable Machine Image Model</div>
<button type="button" onclick="init()">Start</button>

<div id="webcam-container"></div>
<div id="label-container"></div>

<script src="https://cdn.jsdelivr.net/npm/@tensorflow/tfjs@latest/dist/tf.min.js"></script>
<script src="https://cdn.jsdelivr.net/npm/@teachablemachine/image@latest/dist/teachablemachine-image.min.js"></script>

<script type="text/javascript">
    const URL = "https://teachablemachine.withgoogle.com/models/G7JwVrxNr/";

    let model, webcam, labelContainer, maxPredictions;

    async function init() {
        const modelURL = URL + "model.json";
        const metadataURL = URL + "metadata.json";

        model = await tmImage.load(modelURL, metadataURL);
        maxPredictions = model.getTotalClasses();

        const flip = true;
        webcam = new tmImage.Webcam(200, 200, flip);

        await webcam.setup();
        await webcam.play();

        window.requestAnimationFrame(loop);

        document.getElementById("webcam-container").appendChild(webcam.canvas);

        labelContainer = document.getElementById("label-container");

        for (let i = 0; i < maxPredictions; i++) {
            labelContainer.appendChild(document.createElement("div"));
        }
    }

    async function loop() {
        webcam.update();
        await predict();
        window.requestAnimationFrame(loop);
    }

    async function predict() {
        const prediction = await model.predict(webcam.canvas);

        for (let i = 0; i < maxPredictions; i++) {
            const classPrediction =
                prediction[i].className + ": " +
                prediction[i].probability.toFixed(2);

            labelContainer.childNodes[i].innerHTML = classPrediction;
        }
    }
</script>
