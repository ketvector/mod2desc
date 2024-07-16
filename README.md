# mod2desc
translate pytorch model to natural language descriptions

The notebook file [./mod2desc-v4.ipynb](./mod2desc-v4.ipynb) is annotated and has all descriptions, still copying the high level overview below.

-------------------------------------

The requirements are not precise so i will just note down my assumptions at the top here and also call them out as part of the comments in code:

1. What should be the **synthetically created** input and output of the model ?

- **Input** : What should be the input to the model ? 

	The doc says - "(input should be) ... the architecture of a neural network created in PyTorch.This includes detailed information about its layers, configurations, and parameters." 

	There are several options which can be considered here:

	a) serialise the model and use the serialised string as input
	b) use the ouput of `__str__()` method of the model
	c) use a library like https://pypi.org/project/torch-summary/ to create it.

	It will be the toughest for the model to learn from (a) above . (b) & (c) should be comparable.

	For simplicity i have gone with (b) - using `model.__str__()` for input.
 
-  **Output**: What should be the output of the model ?

	A couple of options here are: 

	a) use a large language model to generate the output (openAI api/self-hosted LLAMA)
	b) write a simple function on my own.
		
	For simplicity, I have gone with (b)

2. Which **model** should I use ?
	
	The doc says a "seq2seq" model. The term seq2seq is most often used in the context of a RNN (encoder + decoder) based architecture, with or without attention. Although some people also use it in the context of transformer based architecture, but that is rare.
	
	I have gone with RNN based encoder/decoder arch with attention and adapted my model from here: https://pytorch.org/tutorials/intermediate/seq2seq_translation_tutorial.html
    
    The choice of seq2seq model means that it typically performs well with input sizes of about 30-50 characters. The sizes I have taken are longer but the model performs reasonably well.
