### Transformer Architecture

### Embedding

Text is split into words (token) and tokens with similar meaning are close together in vector space.

### Positional Encoding

Because process are happening in parallel, the model dont know which token is in which position. It adds a positional vector of the same dimension to each token embedding using sine and cosine waves so the model distinguishes "Dog bites man" from "Man bites dog."

### Self Attention

Self-attention lets each token look at every other token in the sequence to gather context (for example, figuring out that "it" refers to "the animal" in "The animal didn't cross the street because it was too tired").
Each token embedding is projected into three learned vectors via weight matrices (Q,V,K):
Query: "What am I looking for?" (the current token's search request)
Key: "What do I contain?" (each token's searchable label)
Value: "What information do I pass along if matched?" (each token's actual content)
Use softmax function.

### Multi-Head Attention

Instead of running one attention calculation, the model splits $Q, K, V$ into parallel attention heads, computes attention independently in each head, and concatenates the results.

### Residual Connections (Add) & Layer Normalization (Norm)

Residual (Add): Shortcut connections let gradients flow directly backward through deep stacks without vanishing, while preserving the original token identity.
Layer Norm (Norm): Normalizes activations across features per token to keep training stable and fast.

### Decoder

The Decoder is the generative half of the Transformer. While the Encoder reads and understands the entire source input all at once, the Decoder generates the target output autoregressively—one token at a time, left to right—feeding each newly predicted token back into itself until it outputs an [EOS] (End-of-Sequence) token.

### Differences between other architecture

In RNN, the process must be in sequence step by step which takes time. In transformer, as the process can be in parallel which is a lot faster. Also in RNN, signals from early words weaken across many time steps, causing vanishing gradients. But in transformer any word interacts directly with any other word in a single attention step, regardless of distance. Also in RNN, entire past history is squeezed into one hidden state vector whereas in transformer every token builds a custom context vector weighted toward the most relevant tokens in the sequence.
