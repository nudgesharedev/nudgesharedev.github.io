# 8. Machine Learning Research Part 1

In our project, we’ll need to use machine learning in several places. Firstly, for the conversational AI / medial chat bot, we’ll need something which asks relevant questions and can identify the subjects of answers. Secondly, when analysing a users health data to make future predictions, we’ll need to employ statistical forecasting methods.

In this article, we go over our initial research into machine learning (generally).

## Supervised Learning

Supervised learning is learning a general rule from a number of labelled examples. A reasonable analogue is how infants learn the alphabet.

As an infant, you're shown many examples of alphabet letters along with their labels. For example, a mother may show her child cards with letters on them while vocalising each letter's name.

After a while the child begins to form abstract models of what makes something an A or a B or a C etc. With this, they can classify any letter, no matter the specific font, size or colour.

![Untitled](8%20Machine%20Learning%20Research%20Part%201%203596b4b71d734722a0686cf855ffeb19/Untitled.png)

We can perform virtually the same task with a computer and teach it to recognise letters, numbers, cats, human faces etc. 

Supervised learning also works for non-visual things. For example, predicting whether a student will receive an A+ based on their hours of study and hours of sleep. The machine would need to be given examples of students who received A+ grades and how long they studied and slept.

## Approximation

While it may seem strange, recognising letters and predicting grades are actually the same problem. How? Well consider the questions being asked:

- How do the pixels of an image relate to it being a certain letter?
- How do a student's sleep and study hours relate to their grade?

We're always trying to find an unknown relationship between inputs (features) and outputs (labels). We're just approximating a function.

![Untitled](8%20Machine%20Learning%20Research%20Part%201%203596b4b71d734722a0686cf855ffeb19/Untitled%201.png)

Thinking about supervised learning as an approximation problem is very powerful because it allows us to use existing maths from calculus and probability theory.

## Models

While the relationship between features and labels may be unknown, we can usually make some general assumptions. These assumptions form a statistical model.  For example, we might guess that a student's grade has a linear relationship to their hours of sleep and study. 

Usually plotting data can help us tell which models might work.

![A linear model is viable for this dataset](8%20Machine%20Learning%20Research%20Part%201%203596b4b71d734722a0686cf855ffeb19/Untitled%202.png)

A linear model is viable for this dataset

## Learning Parameters

Consider this dataset with one feature and one label:

```
x    y
2    5
3    7
6    9
4    7
```

We'll assume this relationship is linear, meaning it has the form:

## $y = mx + c$

This model has two parameters, $m$ and $c$. To find their optimal values, we can just draw a LOBF through a scatter graph of the data. However, in machine learning, models can have millions of parameters.

Luckily, we can use optimization algorithms like gradient descent to do the number crunching for us. Training a model means finding which parameters best fit the given data.