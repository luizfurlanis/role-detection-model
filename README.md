## Treinamento do modelo YOLO

### 1. Instalar as dependências

Instale o Ultralytics (recomendado usar um ambinete virtual):

```bash
python -m venv venv
source venv/bin/activate

python -m pip install ultralytics
```

Verifique se a instalação foi concluída:

```bash
yolo checks
```

### 2. Estrutura da pasta

O dataset deve seguir a estrutura abaixo:

```text
.
├── data.yaml
├── train/
│   ├── images/
│   └── labels/
├── valid/
│   ├── images/
│   └── labels/
└── test/
    ├── images/
    └── labels/
```

O arquivo `data.yaml` deve conter os caminhos corretos para os conjuntos de treinamento, validação e teste, além dos nomes das classes.

Exemplo:

```yaml
train: train/images
val: valid/images
test: test/images

nc: 1
names:
  - buraco
```

**Importante:** utilize os valores reais do `data.yaml` fornecido pelo dataset. O exemplo acima pressupõe uma única classe chamada `buraco`.

### 3. Executar o treinamento

Com o ambiente virtual ativado e o dataset configurado, execute:

```bash
yolo detect train \
    model=yolo11n.pt \
    data=data.yaml \
    epochs=100 \
    imgsz=640 \
    batch=-1
```

Parâmetros utilizados:

| Parâmetro | Descrição |
|---|---|
| `model=yolo11n.pt` | Modelo YOLO11 nano, indicado para um primeiro experimento. |
| `data=data.yaml` | Configuração do dataset. |
| `epochs=100` | Número máximo de épocas de treinamento. |
| `imgsz=640` | Resolução utilizada no treinamento. |
| `batch=-1` | Solicita ao Ultralytics que estime automaticamente o tamanho do lote conforme a memória disponível. |

Na primeira execução, o modelo pré-treinado será baixado automaticamente. É necessária uma conexão com a internet.

### 4. Localizar o modelo treinado

Por padrão, os resultados são salvos em:

```text
runs/detect/train/
```

O principal arquivo gerado é:

```text
runs/detect/train/weights/best.pt
```

Esse arquivo contém os pesos da melhor versão do modelo observada durante a validação.

### 5. Avaliar o modelo

Para avaliar o modelo no conjunto de teste, execute:

```bash
yolo detect val \
    model=runs/detect/train/weights/best.pt \
    data=data.yaml \
    split=test \
    imgsz=640
```

A avaliação apresenta métricas como precisão, recall, mAP50 e mAP50-95.

### 6. Testar o modelo em uma imagem

Para realizar uma predição em uma imagem:

```bash
yolo detect predict \
    model=runs/detect/train/weights/best.pt \
    source=caminho/para/imagem.jpg \
    imgsz=640 \
    save=True
```

Substitua `caminho/para/imagem.jpg` pelo caminho de uma imagem real.

As imagens processadas serão salvas, por padrão, em um diretório dentro de `runs/detect/predict/`.