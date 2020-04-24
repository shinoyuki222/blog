---
layout: post_layout
title: Word Representations
date: 2018-05-01
location: Edinburgh
category: [Natural Language Processing]
tag: [NLP, ML, DL]
pulished: true
mathjax: true
---

### Word representations and morphology

#### Deep learning is not magic and will not solve all of your problems, but representation learning is a very powerful idea.
- NNs don't reuse other informations, only use the input info.
- Human beings often use other informations, we learn language to solve other problems.
- Word embedding: if we learn a matrix, can keep matrices to initialise some other problem.

#### Word representations can be transferred between models.
- word representations pick up all associations: some are useful to applications; some are harmful for people to building a system (e.g., bias problem).
 

\subsubsection*{Word2vec trains word representations using an objective based on language modeling--so it can be trained on unlabeled data.}

\begin{itemize}
  \item we could use POS to training the word representations: Feedforward is feasible, could conditioned on entire input sentence.
  \item using an objective based on language modeling, therefore, it can be trained on unlabeled data.
  \item training a LM is supervised learning problem: it predicts a word conditioned on the context, all you need for supervised learning are examples of context words

  \item idea of CBOW (continuous bag-of-words)
    \begin{itemize}
      \item CBOW adds inputs from words within short window to predict the current word
        \begin{itemize}
          \item predict a word based on context.
          \item do not want to predict the prob. of a string (sentence)
          \item not a generative model of a text: independent senses?????
        \end{itemize}
      \item The weights for different positions are \textbf{shared}
      \item computationally much more efficient than normal NNLM
      \item The hidden layer is just linear
    \end{itemize}
  \item Skip-gram: 
    \begin{itemize}
      \item reformulate the CBOW model (opposite to CBOW) by predicting surrounding words (context words, a word nearby) using the current word.
      \item Find word representations usful for predicting surrounding words in a sentence or document.
      \item each word have two vector representations associated with it: \textit{word representation vector} and \textit{output weight vector}
      \item Learning skip-gram: \textit{see below}
    \end{itemize}

\begin{figure}[ht]
  \centering
    \subfigure[CBOW architecture]{\label{fig:cbow}\includegraphics[width=0.3\linewidth]{figures/cbow.png}}
    \subfigure[The Skip-gram model architecture]{\label{fig:skip}\includegraphics[width=0.3\linewidth]{figures/skip.png}}
  \caption{Model}
  \label{fig:wr}
\end{figure}

\end{itemize}

\subsubsection*{Sometimes called unsupervised, but objective is supervised!}
\begin{itemize}
  \item the ideal context word is given, we don't care about supervision
\end{itemize}

% \subsubsection*{Vocabulary is not finite.-- problem for word2vec}
% \begin{itemize}
%   \item fixed number of word, fixed vocab size
%   \item subword representation, character-level
% \end{itemize}

\subsubsection*{Compositional representations based on morphemes make our models closer to open vocabulary.}
\begin{itemize}
  \item more generalizable, predict (represent) unseen words better

\end{itemize}

\subsubsection*{Limitations of word2vec}
\begin{itemize}
  \item fixed number of word, fixed vocab size, but \textbf{Vocabulary is not finite}.
  \item cannot exploit \textit{functional relationships} (syntactic relationship): 

  e.g, we could learn ``cat''-``cats'' = ``dog''-``dogs'', but if we haven't seen ``sloths'' before, we can't capture ``sloth''-``sloths'' = ``cat''-``cats''. That is, we can't learn \textit{functional relationships} from context.
\end{itemize}

\subsubsection*{Learning skip-gram}
\begin{itemize}
  \item SGD and BP
  \item sub-sample the frequent words (e.g., ``the'', ``is'', ``a'')
  \item words are thrown out proportinal to their frequency (make things faster, reuses importance of frequent words)
  \item non-linearity does not seem to imporve performance of these models, thus the hidden layer dose \textbf{not} use \textbf{activation function}
  \item problem: very large output layer with size = vocab. size, can easily be in order of millions $\rightarrow$ too many outputs to evaluate
  \item solution: negative sampling, Hierarchical softmax
\end{itemize}

\subsubsection*{Negative sampling}
\begin{itemize}
  \item Instead of propagating signal from the hidden layer to the whole output layer, only the output neuron that represents the positive class and few randomly sampled neurons are evaluated
  \item e.g., word pair `cat sat', input `cat', context word (output is 1) `sat', output neuron correspond to `sat' is positive, others negative. we sample other, say 5, negative neuron to evaluate. so only calculate 1+5 = 6 neurons. For word representation vector, we \textbf{always only} update the one corresponding to the input word (it's true whenever we use negative sampling or not).
\end{itemize}

