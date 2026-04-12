<h2>April 12</h2>
<p>I came across a <a href="https://medium.com/@weiwen21/an-image-is-worth-16x16-words-transformers-for-image-recognition-at-scale-957f88e53726">summary of a research paper</a>a called <mark>An Image is Worth 16x16 Words: Transformers for Image Recognition at Scale</mark>. This paper suggests using trasformers in computer vision problems and compares the approach to the classic CNN.
Since I have experience in using transformer based models, it took me no time to understand the general idea of incorporating transformers, which is typically used in NLP, into image classification. Still, it amazed me how the authors are able to think outside of the box and see the overlapping identities that both natural language and images are sequential data.
From the summary, the underlying idea of ViT is <mark>"Split the image into 16*16 patches" then treat each patch as an embedding of a token</mark>.
In a typical transformer based model, the token is first mapped to an embedding followed by positional encoding, then fed into mulitple transformer layers to result in a context aware vectors. 
Because I learnt about transformers through a text classification problem using a Bert-based model, the classification is done on the pooling vector of the [CLS] token.
I would assume that ViT does something similar where the is a particualr token that learns about the summary of the whole image but this is not mentioned in the summary article.
As mentioned in the article, "The last visualization shows that the ViT is able to look at local and global features in lower layers". Unlike CNN, which is built based on the relevance of nearby pixels, ViT is able to look at the big picture.
Also, I am curious what will the optimal structure that the paper suggests and more ways to evaluate a model (they have some interesting graphs). Therefore, I took some time to read through the paper.</p>

<p>2d interpolation is basically torchvision.transforms.Resize</p>
