# Object Detection with YOLOv4

Este repositório contém a implementação de um modelo de **Detecção de Objetos** utilizando o **YOLOv4** (You Only Look Once), que é uma arquitetura eficiente para detecção em tempo real. O objetivo deste projeto é treinar um modelo YOLO para detectar objetos personalizados em imagens, como "pessoa" e "carro", usando a framework **Darknet**.

## Features

- Detecção de objetos em tempo real com a arquitetura **YOLOv4**.
- Treinamento de modelo customizado com objetos específicos (como "pessoa" e "carro").
- Arquivos de configuração e dados prontos para uso.
- Compatível com o Google Colab para treinamento com GPU.

## Requisitos

Antes de rodar o código, certifique-se de ter as seguintes dependências instaladas:

- **Google Colab** (para rodar em GPU)
- **CUDA** e **cuDNN** (para aceleração via GPU)
- **OpenCV** (para pré-processamento e visualização de imagens)
- **gdown** (para download de arquivos do Google Drive)
- **Darknet** (repositório oficial de YOLO)

## Como Usar

1. **Clonar o Repositório**

   Clone o repositório para o seu ambiente local ou para o Google Colab:

   ```bash
   git clone https://github.com/SEU-USUARIO/SEU-REPOSITORIO.git
   ```

2. **Montar o Google Drive (se estiver usando Colab)**

   No Google Colab, você pode montar o Google Drive para armazenar os arquivos:

   ```python
   from google.colab import drive
   drive.mount('/content/drive')
   ```

3. **Configuração do Ambiente**

   Configure a **GPU**, **CUDNN** e **OpenCV** em **Darknet**:

   ```bash
   !sed -i 's/OPENCV=0/OPENCV=1/' Makefile
   !sed -i 's/GPU=0/GPU=1/' Makefile
   !sed -i 's/CUDNN=0/CUDNN=1/' Makefile
   !sed -i 's/CUDNN_HALF=0/CUDNN_HALF=1/' Makefile
   ```

4. **Baixar e Preparar os Dados**

   Baixe os arquivos de treinamento (imagens e rótulos) com **gdown** e extraia-os:

   ```bash
   !gdown --id SEU-ID-IMAGEM -O data/obj/images.zip
   !gdown --id SEU-ID-ROTULOS -O data/obj/labels.zip
   !unzip data/obj/images.zip -d data/obj/
   !unzip data/obj/labels.zip -d data/obj/
   ```

5. **Treinamento do Modelo**

   Inicie o treinamento do modelo YOLOv4 com a configuração ajustada:

   ```bash
   !./darknet detector train data/obj.data cfg/yolov4-obj.cfg yolov4.conv.137 -dont_show -map
   ```

   **Observação:** O arquivo `yolov4.conv.137` é um arquivo de pesos pré-treinado que ajuda a acelerar o processo de treinamento.

6. **Inferência com o Modelo Treinado**

   Após o treinamento, você pode testar o modelo com uma imagem de entrada. Primeiro, baixe a imagem de teste:

   ```bash
   !gdown --id SEU-ID-IMAGEM-TESTE -O test_image.jpg
   ```

   E então, execute a inferência:

   ```bash
   !./darknet detector test data/obj.data cfg/yolov4-obj.cfg backup/yolov4-obj_last.weights test_image.jpg -thresh 0.5 -outpredictions predictions.jpg
   ```

7. **Visualizar os Resultados**

   Por fim, visualize a imagem resultante da inferência:

   ```python
   import matplotlib.pyplot as plt
   import cv2

   result_image = cv2.imread('predictions.jpg')
   if result_image is not None:
       result_image = cv2.cvtColor(result_image, cv2.COLOR_BGR2RGB)
       plt.figure(figsize=(10, 10))
       plt.imshow(result_image)
       plt.axis('off')
       plt.title("Resultado da Inferência")
       plt.show()
   else:
       print("Erro: O arquivo 'predictions.jpg' não foi gerado. Verifique a execução da inferência.")
   ```

## Estrutura de Arquivos

A estrutura do repositório é a seguinte:

```
/data
    /obj
        /images
        /labels
    /train.txt
    /test.txt
/backup
/cfg
    yolov4-obj.cfg
    yolov4-custom.cfg
/weights
    yolov4.conv.137
/README.md
```

## Contribuições

Se você encontrar problemas ou tiver melhorias para sugerir, sinta-se à vontade para abrir um **issue** ou enviar um **pull request**.

