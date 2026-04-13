<h2>Introduction</h2>
<p>I came across a <a href="https://medium.com/@weiwen21/an-image-is-worth-16x16-words-transformers-for-image-recognition-at-scale-957f88e53726">summary of a research paper</a>a called <mark>An Image is Worth 16x16 Words: Transformers for Image Recognition at Scale</mark>. This paper suggests using trasformers in computer vision problems and compares the approach to the classic CNN.
Since I have experience in using transformer based models, it took me no time to understand the general idea of incorporating transformers, which is typically used in NLP, into image classification. Still, it amazed me how the authors are able to think outside of the box and see the overlapping identities that both natural language and images are sequential data.
From the summary, the underlying idea of ViT is <mark>"Split the image into 16*16 patches" then treat each patch as an embedding of a token</mark>.
In a typical transformer based model, the token is first mapped to an embedding followed by positional encoding, then fed into mulitple transformer layers to result in a context aware vectors. 
Because I learnt about transformers through a text classification problem using a Bert-based model, the classification is done on the pooling vector of the [CLS] token.
I would assume that ViT does something similar where the is a particualr token that learns about the summary of the whole image but this is not mentioned in the summary article.
As mentioned in the article, "The last visualization shows that the ViT is able to look at local and global features in lower layers". Unlike CNN, which is built based on the relevance of nearby pixels, ViT is able to look at the big picture.
Also, I am curious what will the optimal structure that the paper suggests and more ways to evaluate a model (they have some interesting graphs). Therefore, I took some time to read through the paper.</p>

<h4>Important takeaways from the paper</h4>
<ul>
<li>2d interpolation is basically torchvision.transforms.Resize</li>
<li>the patches flattened then linearly transformed (equivalent to doing a Conv2d with stride=P, kernel_size=P)</li>
<li>a hybrid model refers to applying ViT on feature maps from CNN</li>
<li>models experimented:</li>
<ol>
  <li>ViT: Base(12 layers, 12 attention heads per layer), Large(24 * 16), Huge(32 * 16)</li>
  <li>CNN: ResNet, EfficientNet</li>
  <li>hybrid: ResNet50 (with stage 4) -> ViT, REsNet50 (without stage 4) - ViT</li>
</ol>
<li>attention distance: sum of attention weight * physical distance, small means that the block is focusing on local(nearby) tokens</li>
  
![attention distance graph](https://github.com/ytrqua3/ViT_paper_replicating/blob/322ab0b1e8944679753719bfae7e803dc009524e/attention_distance_graph.png)
<li>The graph shows that the ViT learns both locally and globally in lower layers, then gradually becomes more global as the model goes deeper</li>
<li>The graph also shows that for a hybrid model, the lower layers learn less globally, deducing that the CNN does the local learning before passing into ViT</li>
<li>This behavior is similar to CNN where the receptive field grows as the models goes deeper</li>

![attention on each patch](https://github.com/ytrqua3/ViT_paper_replicating/blob/9c0ecc6bdef7c5871e058ae3454c22c9a958b450/6.png)
<li>These figures show the attention on each patch according to the attention matrix and the model attends to regions that are helpful for classification</li>
<li>Both globally average pooling and class token work</li>
<li>positional embeddings are trainable parameters unlike some transformer models that use fixed sinusoidal functions</li>
</ul>

<h4>Structure of the model</h4>
<ol>
  <li>Split the image into 16x16 patches -> Flatten -> trainable linear projection -> add learnable positional encoding</li>
  <li>Transformer Encoder:</li>
  <ol>
    <li>Multihead self attention</li>
    <li>Multilayer perceptron: "two layers with a GELU non-linearity"</li>
  </ol>
  <li>Classifier</li>
</ol>

![notes p.1](https://github.com/ytrqua3/ViT_paper_replicating/blob/913129242fa5dee25b8be47eff428b9099097fd5/ViT_notes1.jpeg)

![notes p.2](https://github.com/ytrqua3/ViT_paper_replicating/blob/913129242fa5dee25b8be47eff428b9099097fd5/ViT_notes2.jpeg)

![ViT structure](https://github.com/ytrqua3/ViT_paper_replicating/blob/b14c4ac9367c4ceb02cad55014cc5d1ed7990930/ViT_summary.PNG)
