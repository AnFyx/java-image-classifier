# Java Image Classifier: cat / dog / wild animal

Image classification with single-layer neurons implemented from scratch in pure Java (Java SE only, no external libraries).
Team project, ISEN Méditerranée engineering school, 2026. Code identifiers, comments and documents are in French.

## Results

Three classes, 10,000 training images, 3,200 test images.

| Input representation | Best test accuracy |
|---|---|
| Raw RGB pixels | 73.5% |
| HSL colour | 71.8% |
| 2D FFT magnitude | 63.1% |
| HOG | 87.4% |
| **HOG + data augmentation** (mirroring + rotation, 4x training set) | **90.0%** |

Limitation: iteration counts and hyperparameters were compared directly on the test split (no separate validation set), so these figures are slightly optimistic.
Raw logs are in `results/`, the full analysis in `docs/rapport_projet_IA_V2.pdf`.

## How it works

- **Features:** raw RGB pixels, HSL colour, 2D FFT magnitude, Histogram of Oriented Gradients (HOG).
- **Classifier:** One-vs-All architecture with one expert neuron per class (cat, dog, wild).
- **Neurons:** Heaviside, sigmoid and ReLU activations behind a common interface (`iNeurone`).
- **Data augmentation:** mirroring, rotation, translation, zoom and noise to artificially enlarge the training set.
- **GUI:** Swing application to train the model, save and load weights, and classify a chosen image.

## Repository structure

```
src/               Swing application: training pipeline, HOG extraction, GUI
experiments/src/   Console experiments: feature benchmarks, data augmentation, kNN and noise tests
results/           Raw benchmark logs
docs/              Final report, slides, activation-function studies, task tracker (French)
modele_*.txt       Saved weights of the three expert neurons (loaded by the GUI)
```

## Running

No build tool. Compiles with JDK 23. Run all commands from the repository root: the dataset path and the weight files are resolved relative to the working directory.

```bash
javac -encoding UTF-8 -d out src/*.java
java -cp out MainProjet
```

Experiments (classes are in package `src`):

```bash
javac -encoding UTF-8 -d out-exp experiments/src/*.java
java -cp out-exp src.MainProjet
java -cp out-exp src.TestKNN
java -cp out-exp src.TestBruit
java -cp out-exp src.Image
java -cp out-exp src.testNeurone
```

## Dataset

The dataset was provided by ISEN Méditerranée for the course and is not redistributed here. Expected layout:

```
dataset_groupe_8/
├── train/{cat,dog,wild}/*.jpg
└── test/{cat,dog,wild}/*.jpg
```

All images must have the same dimensions: images whose size differs from the first loaded image are skipped.

## Team and my contribution

Group project by four students: [@AnFyx](https://github.com/AnFyx) (Thomas Iliot), [@Blue8340](https://github.com/Blue8340), [@Sinay0543](https://github.com/Sinay0543), [@JuanitoPepers](https://github.com/JuanitoPepers).

My part (Thomas Iliot) was model improvement:
- identified and tested techniques to raise accuracy, and ran the benchmarks comparing input representations (raw RGB, HSL, FFT, HOG);
- implemented data augmentation (mirroring, rotation, translation, zoom, noise); HOG combined with augmentation gave the best result (90.0%);
- wrote the kNN comparison and noise-robustness tests (`experiments/src/TestKNN.java`, `experiments/src/TestBruit.java`);
- restructured the codebase and maintained the project task tracker.

No license: all rights reserved by the authors.
