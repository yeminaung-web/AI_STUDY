1. Why does an Inception module use convolutional filters of different sizes in parallel?

In inception module, we need different size of filter n order to capture features at multiple spatial scales simultaneously. For example small filter like 1x1 and 3x3 capture detailed textures and edges while 5x5 capture larger broader patterns and context. 

2. Why is it necessary to extract information at multiple scales from an image or feature map?
   
Because different distance from camera, perspective or framing has different pixel area and if we only used fixed kernel for example if we only use small kernel which will miss broader context and if we only use large kernel, the detail will be missed. 

3. What role does 1x1 convolution play in an Inception module?

   1x1 is mainly use to reduce the channel depth which is also called bottleneck. It is placed before 3x3 and 5x5 to reduce the overall channel depth and mix information across channels and add extra ReLU non-linearity. Another thing is 1x1 is also placed after 3x3 max pooling to reduce the channel depth before concatenation because pooling produce the full channel depth of the input. 

4. How does $1 \times 1$ convolution reduce computational cost?
   
Let’s suppose we want to reduce 28 x 28 x 192 to 28 x 28 x 32 and we will be using kernel size 5 x 5.  Without 1x1 filter, 5x5x32 filter would be weights: 5x5x192x32 =153,600 and FLOPS: 28x28x153,600 = 120.4 M. With 1x1 filter, first step 1x1x16, weights: 1x1x192x16 =3,072, FLOPS: 28x28x3072 = 2.4M and then, step 2 5x5x32 filter, weights: 5x5x16x32=12,800 and FLOPS: 28x28x12800=10M. So total would be weights = 3072+12800 =15,872 and FLOPS = 2.4 + 10 = 12.4M. So we can check that without 1x1 filter there will be 153,600 parameters and FLOPS 120.4M but with 1x1 filter, there will only be 15872 parameters and FLOPS 12.4M which is 10 times reduced.

5. How was GoogLeNet able to maintain a relatively small number of parameters despite its depth?

As I mentioned above by using 1x1 filter, we can greatly reduced the parameter 10 times smaller and prevent parameter explosion as the network grew deeper and wider. Another thing is GoogLeNet use Global Average Pooling (GAP) rather than flattening the 7x7x1024 into massive dense layer, it average 7x7 into a single scalar which is 1x1x1024 before one lightweight linear classifier.

6. What are the advantages and limitations of an Inception module?

   Dimension	Advantages	Limitations
Feature Extraction	Captures multi-scale patterns (1 \times 1, 3 \times 3, 5 \times 5, and pooling) simultaneously within the same layer.	Over-compressing channels in 1 \times 1 bottlenecks can cause representational information loss.
Architecture Design	Removes the need to manually pick a single optimal kernel size at each network stage.	Requires tuning 6 separate filter counts per module, making scaling and customization complex.
Compute & Hardware	1 \times 1 bottlenecks slash parameters and FLOPs, enabling deeper and wider models.	Parallel branches and tensor concatenation create memory overhead, reducing GPU efficiency.
Network Depth	Extra 1 \times 1 layers add ReLU non-linearities without increasing spatial filter size.	Lacks identity skip connections (in Inception-v1), leaving very deep stacks prone to gradient degradation.
