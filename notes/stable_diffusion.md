![image](https://github.com/user-attachments/assets/8a4412f1-5259-43a8-a7f9-8596342b976f)


With this we come to see the three main components (each with its own neural network) that make up Stable Diffusion:

ClipText for text encoding.
Input: text.
Output: 77 token embeddings vectors, each in 768 dimensions.

UNet + Scheduler to gradually process/diffuse information in the information (latent) space.
Input: text embeddings and a starting multi-dimensional array (structured lists of numbers, also called a tensor) made up of noise.
Output: A processed information array

Autoencoder Decoder that paints the final image using the processed information array.
Input: The processed information array (dimensions: (4,64,64))
Output: The resulting image (dimensions: (3, 512, 512) which are (red/green/blue, width, height))



![image](https://github.com/user-attachments/assets/aa1109c2-d42d-4fe7-a356-684e91f4453b)


![image](https://github.com/user-attachments/assets/7ec9d713-12df-4480-b4b6-db07f4e4560f)


Diffusion happens in multiple steps.

![image](https://github.com/user-attachments/assets/5a502cf5-2aa3-4fd2-8105-3f18cebbbb6b)


To speed up the image generation process, the Stable Diffusion paper runs the diffusion process not on the pixel images themselves, but on a compressed version of the image. The paper calls this “Departure to Latent Space”.

This compression (and later decompression/painting) is done via an autoencoder. The autoencoder compresses the image into the latent space using its encoder, then reconstructs it using only the compressed information using the decoder.


The forward process (using the autoencoder’s encoder) is how we generate the data to train the noise predictor. Once it’s trained, we can generate images by running the reverse process (using the autoencoder’s decoder).

![image](https://github.com/user-attachments/assets/022636af-8500-4665-b230-5bf005285315)


## The Text Encoder

A Transformer language model is used as the language understanding component that takes the text prompt and produces token embeddings. The released Stable Diffusion model uses ClipText (A GPT-based model), while the paper used BERT.

CLIP is trained on a dataset of images and their captions. Think of a dataset looking like this, only with 400 million images and their captions.

CLIP is a combination of an image encoder and a text encoder. Its training process can be simplified to thinking of taking an image and its caption. We encode them both with the image and text encoders respectively.


## Src
* https://jalammar.github.io/illustrated-stable-diffusion/
* https://huggingface.co/blog/stable_diffusion




