# Итоговая аттестация: Data Science в медицине

Репозиторий содержит Jupyter-ноутбук для итоговой работы по классификации изображений кожных поражений.

## Главный ответ про датасет 12 ГБ

Датасет **не нужно и нельзя загружать в GitHub**. GitHub хранит код, а большой датасет должен лежать отдельно.

Для этой работы рекомендуется запускать ноутбук в **Kaggle Notebook** и подключать датасет как **private Kaggle Dataset**. Подробная инструкция находится в файле [`docs/RUN_12GB_DATASET.md`](docs/RUN_12GB_DATASET.md).

## Если Kaggle не даёт включить Internet

Это нормально: интернет в Kaggle **не обязателен** для запуска проекта.

Чтобы сохранить требование «хотя бы одна pretrained-модель», заранее загрузите в Kaggle отдельный private Dataset с весами `torchvision`:

- `resnet18-f37072fd.pth`;
- `mobilenet_v3_large-8738ca79.pth`.

Их можно скачать на компьютере с доступом в интернет по официальным ссылкам PyTorch:

```text
https://download.pytorch.org/models/resnet18-f37072fd.pth
https://download.pytorch.org/models/mobilenet_v3_large-8738ca79.pth
```

После этого подключите к Kaggle Notebook **два Dataset** через **Add data**:

1. Dataset с изображениями кожных поражений.
2. Dataset с `.pth`-весами.

Ноутбук по умолчанию работает в режиме `PRETRAINED_WEIGHTS_MODE = "local"`, поэтому не будет пытаться выходить в интернет и найдёт веса в `/kaggle/input`.

## Что внутри

- `notebooks/skin_lesion_classification_final.ipynb` — end-to-end ноутбук под финальную работу.
- `docs/RUN_12GB_DATASET.md` — пошаговая инструкция, куда загрузить датасет 12 ГБ и где запускать обучение.
- `requirements.txt` — минимальные зависимости для локального запуска.

Ноутбук включает:

- автоопределение путей для локального запуска, Kaggle и Colab;
- загрузку данных из папок классов или `metadata.csv`;
- offline-загрузку ImageNet-весов из локальных `.pth`-файлов;
- проверку изображений;
- EDA;
- resize, нормализацию и аугментации;
- train/validation/test split;
- `Dataset` и `DataLoader`;
- обучение ResNet18 и MobileNetV3 с ImageNet-весами;
- fine-tuning лучшей модели;
- сравнение экспериментов;
- test-оценку;
- confidence threshold analysis;
- визуальную проверку предсказаний.

## Быстрый запуск на Kaggle без Internet

1. Скачайте датасет с Cloud Mail на компьютер.
2. Создайте на Kaggle приватный Dataset с изображениями.
3. Создайте на Kaggle второй приватный Dataset с файлами весов `resnet18-f37072fd.pth` и `mobilenet_v3_large-8738ca79.pth`.
4. Создайте Kaggle Notebook.
5. Подключите оба Dataset через **Add data**.
6. Включите GPU: **Settings → Accelerator → GPU**.
7. Оставьте **Internet выключенным**.
8. Загрузите `notebooks/skin_lesion_classification_final.ipynb`.
9. Для первого теста задайте `SAMPLE_PER_CLASS = 200`, для финального запуска — `SAMPLE_PER_CLASS = 0`.
10. Запустите ноутбук сверху вниз и сохраните `.ipynb` с outputs.

## Минимальные зависимости для локального запуска

```bash
pip install -r requirements.txt
```

## Важное медицинское ограничение

Полученная модель не должна использоваться как самостоятельный инструмент постановки диагноза. Корректный сценарий — система поддержки принятия решений врача с обязательной экспертной проверкой неуверенных и клинически рискованных случаев.
