#   
Chat GPT in GPT means Generative Pretrained Transformer.   
Many different transformer such as text to image, text to voice  
Transformer started in 2017 used for translation. The text will be predicted repeatedly. So for example. Let’s suppose 저는 is added to the transformer and transformer predicted next word 학생 so the sentence will look like 저는 학생 Then 저는학생 will be added to the transformer and transformer will predict 입니다. So these ai models do One word at a time.  
  
First input are separated into token. If the input is text, the text will be separated into words.   
Words embedding  
The text input will be multiply by the weight and for example let’s go EOS first let’s will be 1 and go and EOS will be 0. So only let’s 1 will multiply by the weight and the activation function will be f(x) = x. The weights are determined by backpropagation.   
![Also, just be clear, the process of](Attachments/B36742AF-40DE-451E-B4C7-C6EBE6572D14.jpeg)  
Backpropagation  
Y = ax + b  
The slope shift until it found the optimal value. Also in neural network the weight stat with random number but when we train the transformer with English and Korean words. Backpropagation optimize one step at a time to get final weights.  
![...and then, using an iterative process, Backpropagation](Attachments/949E87D6-C5A7-4BC4-AC4E-56298EB3B337.jpeg)  
  
Position encoding  
For example, if we input “professor eat pizza” the model have to keep track of word order. Using sin and cosin squiggle. There are different position encoding.   
![Now we just do the math to get](Attachments/7498585E-7ABE-440F-804B-C6540348B374.jpeg)  
Self attention is for example “the pizza is out of oven and it is delicious” but the machine does not know it means pizza or oven. So self attention is compare the first word “the” with the rest of the word and itself. And it continues.   
  
For example if we compare the words “let’s” itself and “go”. We multiply position embedding values of “let’s” with weights to get query.  
Then we will create two key of “let’s” by multiplying position embedding values with weight. Then, we will create two keys for “go” with same weight. Then we will multiply “let’s” query and “let’s” key and added. It is the same for “go”. We will use dot product similarity. Compare the two new value and we can see 11.7 is bigger that -2.6 so let’s is more similar itself than “go”. Then use softmax function. Since “let’s” is more similar itself than “go” we will use 100% of “let’s” to encode “let’s” and 0% of “go” to encode “let’s”. So we create two more values of “let’s” and “go” which we will call “values” and multiply softmax value accordingly and added. Then we got two self attention values.  
![Value ES](Attachments/BD0CF9F9-0E74-4C69-BEC1-B0671FDA028D.jpeg)  
For “go” we can just create query for “go” and do the same process. Weights are different but same for each type.  
![Value PS](Attachments/E2829A4A-9C1C-4C63-93AD-A107DA71330A.jpeg)  
We can call weight, queries, keys and values as attention cell. 8 self attention cell is called multi head attention.  
  
We add position encoding value to the self attention value. We got residual connection value.  
![...by allowing the Self-Attention](Attachments/9C4F07EB-8705-4796-84CC-D183F5B5A8AD.jpeg)  
Second part (Decorder)  
Self attention.  
Start with EOS token. Same process. Then do residual connection value.  
![NOTE: The sets of Weights we](Attachments/C2DD2941-78C8-4EED-BF31-AEAD44751B23.jpeg)  
Encoder-Decoder-Attention  
We got attention value then we multiply attention value of EOS with the weight to get query.  
Multiply attention value of “let’s” and “go” with the same weight to get key. Then we multiply query with keys and run softmax. Then we will know which word to be translated first.  
![14.37. -1.01](Attachments/3BE103AB-8097-47E4-8A40-2CD66BED5614.jpeg)  
Then we create two new values and process with softmax value to get encoder-decoder attention value. The sets of weight we use in encoder-decoder-attention is different with self attention.  
![Now, to make room for the](Attachments/FE9B78F1-ABC9-40C0-B329-3BDEFA2AAAF2.jpeg)  
Then add previous residual connection value and new attention value to get new residual connection values.  
![Encoder](Attachments/60039214-C3C1-4139-8039-74DF77FC8C38.jpeg)  
Then we pass through the final two number to the fully connected layer. Since the total output is 4, the residual connection values will be multiply by weight and added bias. Then we will do softmax function to select the first output word.   
![So far, the translation is correct,](Attachments/1756FB7A-A594-4DCC-866E-0EDC858DE73E.jpeg)  
The decoder won’t stop until it select EOS as output. Then the same process start with vamos.  
![Value 128](Attachments/D46FD269-395E-4AD4-B58D-DEFA1D1D7B2D.jpeg)  
