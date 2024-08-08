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
