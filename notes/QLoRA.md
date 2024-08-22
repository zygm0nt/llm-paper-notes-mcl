## Gist

A quote from  https://www.answer.ai/posts/2024-03-06-fsdp-qlora.html#fsdp-scale-training-to-multiple-gpus:

> QLoRA is a simple but brilliant combination of two critically important advances in modern neural networks: quantization, and LoRA. Quantization is a technique where, instead of using 16 or even 32 bits to store the weights of a neural network, 4 (or even fewer) bits are used. There are only 16 possible values of a 4 bit number, but Dettmers and Zettlemoyer showed that this can be enough in the large language models that are popular today. Tim Dettmers made these 4-bit “quantized” models easy to create, thanks to his bitsandbytes library, and recently Hugging Face has stepped in to help maintain and document this library, particularly thanks to the initiative of Titus von Koeller.
> 
> Unfortunately, once a model is quantized, it can not be trained any further with regular approaches – with just 16 possible values, the gradient descent method used for model training will observe zero gradients nearly everywhere, so it can’t make any updates to the quantized weights. This is a major problem, because it means that quantization can only be used for inference, not for continued pre-training or fine-tuning. Whilst inference is useful and important, it’s really just consuming models. But we want everybody to be able to contribute to creating models!

