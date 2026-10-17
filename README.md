
# CLIP and LLaVA: Image Classification, Retrieval, and Visual Question Answering

## Project Overview

This project explores multimodal AI using two vision-language models: **CLIP** and **LLaVA**. The objective is to understand how these models connect images and natural language, and to evaluate their capabilities in image classification, text-based image retrieval, visual question answering (VQA), and visual hallucination detection.

The project also implements a simple multimodal assistant that combines CLIP image retrieval with LLaVA-generated explanations.

## Objectives

- Perform zero-shot image classification using CLIP.
- Investigate the effect of text prompts on CLIP predictions.
- Retrieve relevant images from a dataset using natural-language queries.
- Use LLaVA to answer questions about image content.
- Evaluate VQA answers and identify visual hallucinations.
- Compare the capabilities and limitations of CLIP and LLaVA.
- Build a simple image retrieval and question-answering pipeline.

## Models Used

### CLIP

**Model:** `openai/clip-vit-base-patch32`

CLIP learns a shared representation of images and text. It can compare an image with candidate text descriptions and rank images according to their similarity to a natural-language query.

In this project, CLIP is used for zero-shot classification and text-based image retrieval.

### LLaVA

**Model:** `llava-hf/llava-1.5-7b-hf`

LLaVA combines a vision encoder with a large language model to generate natural-language responses about images. It is used for image description, visual question answering, and analysis of incorrect or unsupported answers.

## Dataset

The experiments use a collection of images stored in a local directory. The dataset contains examples of animals, people, vehicles, and other visual content.

The images are loaded and processed using the corresponding model processors. No model fine-tuning is performed; the experiments use pretrained models.

## Experiments and Results

### 1. CLIP Zero-Shot Classification

CLIP compares an image with candidate text descriptions and selects the description with the highest image-text similarity.

The experiment evaluated 10 images across five categories:

- Cat
- Dog
- Car
- Airplane
- Horse

**Observed accuracy: 100% on the tested 10-image sample.**

This result applies only to the images used in this experiment and should not be interpreted as a general accuracy guarantee.

### 2. Prompt Engineering

Different text prompts were tested to observe their effect on CLIP similarity scores.

For the image `Greycat.jpg`, the prompt *“a photograph showing a cat”* obtained the highest observed softmax score of 0.806 among the tested candidates.

This illustrates that prompt wording can influence the relative scores produced by CLIP.

### 3. Text-Based Image Retrieval

CLIP image embeddings were computed for the available images. A natural-language query was then encoded into the same representation space, and the images were ranked by similarity.

**Example query:** `a photo of a horse`

| Rank | Retrieved image | CLIP similarity |
|---:|---|---:|
| 1 | `horse.jpg` | 0.3007 |
| 2 | `cheval.jpg` | 0.2791 |
| 3 | `Dasht1.jpg` | 0.2466 |
| 4 | `_Louie_the_Italian_Greyhound_.jpg` | 0.2445 |
| 5 | `18_Year_Old_Windsprite.jpg` | 0.2249 |

The results show that CLIP can rank images according to their semantic relevance to a text query. Similarity scores are not probabilities of correctness.

### 4. Visual Question Answering with LLaVA

LLaVA was evaluated on five images using five questions per image, for a total of 25 questions.

The answers were manually checked against the visible image content.

- Total questions: 25
- Correct answers: 21
- Incorrect answers: 4
- Observed accuracy: 84%

The experiment showed that LLaVA can describe visible objects and answer questions about images, but its responses may contain mistakes or uncertain details.

### 5. Visual Hallucination Analysis

A hallucination test was conducted using an image containing a horse on a grassy hillside.

When asked whether a bicycle was present, LLaVA initially answered that no bicycle was visible. However, when subsequently asked about the bicycle's color and location, it generated details about a black bicycle on the hillside.

Because the image did not contain a bicycle, these additional details were unsupported by the visual evidence.

This example demonstrates that a vision-language model can generate plausible-sounding answers even when the assumed object is absent.

### 6. CLIP and LLaVA Comparison

| Property | CLIP | LLaVA |
|---|---|---|
| Main purpose | Image-text alignment | Vision-language understanding and generation |
| Zero-shot classification | Yes | Possible through prompting |
| Text-based image retrieval | Yes | Not its main purpose |
| Natural-language descriptions | No direct generation | Yes |
| Visual question answering | Not directly | Yes |
| Output | Similarity scores | Generated text |
| Main limitation observed | Depends on candidate descriptions and similarity ranking | Can misinterpret images or hallucinate details |

CLIP is particularly useful for classification and retrieval, while LLaVA is more suitable for generating descriptions and answering open-ended questions.

## Mini-Project: Multimodal Image Assistant

The mini-project combines both models in a single pipeline:

1. **Text query:** The user provides a natural-language description, such as “a photo of a horse”.
2. **CLIP retrieval:** CLIP compares the query embedding with the stored image embeddings.
3. **Top-5 selection:** The five images with the highest similarity scores are retrieved.
4. **LLaVA analysis:** LLaVA receives each retrieved image and answers a question about its visible content.
5. **Results:** The system displays the retrieved images, their similarity scores, and the generated answers.

### Pipeline

`Text Query → CLIP Embedding → Top-5 Image Retrieval → LLaVA Question Answering → Results`

This pipeline demonstrates how image-text retrieval and generative visual question answering can complement each other. However, retrieval relevance does not guarantee that the generated answer is correct.

## Technologies Used

- Python
- Jupyter Notebook
- PyTorch
- Hugging Face Transformers
- CLIP
- LLaVA
- PIL (Pillow)

## How to Run

1. Open the notebook in Jupyter Notebook, JupyterLab, or Google Colab.
2. Install the required dependencies in the execution environment.
3. Configure the image dataset directory.
4. Load the pretrained CLIP and LLaVA models.
5. Execute the notebook cells in order.
6. Review the generated classifications, retrieval results, VQA answers, and hallucination examples.

**Important:** LLaVA 1.5 7B requires substantial GPU memory. A compatible GPU environment or an appropriately configured alternative may be necessary.

## Conclusion

This project demonstrates the complementary capabilities of CLIP and LLaVA in multimodal AI. CLIP achieved 100% accuracy on the small classification test and successfully retrieved images relevant to a text query. LLaVA answered visual questions with an observed manual accuracy of 84%, but the hallucination experiment revealed that it can generate unsupported details.

The combined pipeline illustrates a practical application of vision-language models while highlighting the importance of validating generated responses against actual visual evidence.

## Repository Contents

- `LAB — Vision-Language Models: CLIP & LLaVA.ipynb` — Jupyter notebook containing the experiments, code, outputs, and analysis.
- `README.md` — Project overview, methodology, results, and conclusion.

## Notes

This project was developed for educational purposes. Reported metrics correspond to the specific experiments described in the notebook and do not establish general model performance.
