CRFsuite for partially annotated data
=====================================

This CRF toolkit is a fork of the [CRFsuite: a fast implementation of Conditional Random Fields (CRFs)](https://github.com/chokkan/crfsuite) by enabling it to support ***partially annotated*** input training data.

## Partially annotated data
For sequence labeling problems, we may access to a type of data: the labels for certain subsequence are known or constrainted to a smaller set, while the labels for the other part of the sequence is unknown.
Figure 1 illustrate an example for such type of data.

![image](https://raw.githubusercontent.com/ExpResults/partial-crfsuite/master/.assets/figure1.jpg)

Figure 1

In character-based Chinese word segmentation (which is similar to the named entities recognition that label token with *B*, *M*, *E*, and *S*),  if we know the subsequence of characters makes a word, their labels and labels on the boundary can be constrainted to a smaller set.

In the above example, the word `狐岐山(the Huqi Mountain)` in the unannotated sentence is recognized as a word.
As a result, we obtain a partially-annotated sentence, in which the segmentation ambiguity of the characters `狐(fox)`, `岐(brandy road)` and `山(mountain)` are resolved (`狐` being the beginning, `岐` being the middle and `山` being the end of the same word).
At the same time, the segmentation ambiguity of the surrounding characters `在(at)` and `救(save)` are reduced (`在` being either a single-character word or the end of a multi-character word, and `救` being either a single-character word or the beginning of a multicharacter word).

For more details, please refer to the [our emnlp 2014 paper](#).

## Using partial-crfsuite

### Compiling and running
Since this toolkit is a fork of the [CRFsuite](https://github.com/chokkan/crfsuite), compiling and executating are completely same with the original software.
If you are not quite familiar with CRFsuite, you can refer to its [offical website](http://www.chokkan.org/software/crfsuite/) which provides [fancy document](http://www.chokkan.org/software/crfsuite/manual.html).

### Input data format
The major difference between CRFsuite and partial-crfsuite is that the latter accepts training data with fuzzy or multiple labels.
In the input training data, labels come in the first column as CRFsuite.
To represent the multiple labels, all the possible labels are packed together by a `|` delimiter.
Take the partially annotated sentence in Figure 1 for example, the training data can be
```
e|s	u[-1]=_bos_	u[0]=在	u[1]=狐
b	u[-1]=在	u[0]=狐	u[1]=歧
m	u[-1]=狐	u[0]=歧	u[1]=山
e	u[-1]=歧	u[0]=山	u[1]=救
b|s	u[-1]=山	u[0]=救	u[1]=治
b|m|e|s	u[-1]=救	u[0]=治	u[1]=碧
b|m|e|s	u[-1]=治	u[0]=碧	u[1]=瑶
b|m|e|s	u[-1]=碧	u[0]=瑶	u[1]=，
b|m|e|s	u[-1]=瑶	u[0]=，	u[1]=_eos_
```
